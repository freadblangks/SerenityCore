#
# This file is part of the Serenity Warhammer Online Framework
#
# See LICENSE file for more information
#
# Website: serenitycore.org
# Wiki: wiki.serenitycore.org
# Git: github.com/SerenityCore-Warhammer-Framework
#
# License:
# This program is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License as published by the
# Free Software Foundation; either version 2 of the License, or (at your
# option) any later version.
#
# This program is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for
# more details.
#
# You should have received a copy of the GNU General Public License along
# with this program. If not, see <http://www.gnu.org/licenses/>.

set(protobuf_STAT_SRCS
  src/google/protobuf/compiler/importer.cc
  src/google/protobuf/compiler/parser.cc
  src/google/protobuf/descriptor.cc
  src/google/protobuf/descriptor.pb.cc
  src/google/protobuf/descriptor_database.cc
  src/google/protobuf/dynamic_message.cc
  src/google/protobuf/extension_set.cc
  src/google/protobuf/extension_set_heavy.cc
  src/google/protobuf/generated_message_reflection.cc
  src/google/protobuf/generated_message_util.cc
  src/google/protobuf/io/coded_stream.cc
  src/google/protobuf/io/gzip_stream.cc
  src/google/protobuf/io/printer.cc
  src/google/protobuf/io/strtod.cc
  src/google/protobuf/io/tokenizer.cc
  src/google/protobuf/io/zero_copy_stream.cc
  src/google/protobuf/io/zero_copy_stream_impl.cc
  src/google/protobuf/io/zero_copy_stream_impl_lite.cc
  src/google/protobuf/message.cc
  src/google/protobuf/message_lite.cc
  src/google/protobuf/reflection_ops.cc
  src/google/protobuf/repeated_field.cc
  src/google/protobuf/service.cc
  src/google/protobuf/stubs/common.cc
  src/google/protobuf/stubs/once.cc
  src/google/protobuf/stubs/stringprintf.cc
  src/google/protobuf/stubs/structurally_valid.cc
  src/google/protobuf/stubs/strutil.cc
  src/google/protobuf/stubs/substitute.cc
  src/google/protobuf/text_format.cc
  src/google/protobuf/unknown_field_set.cc
  src/google/protobuf/wire_format.cc
  src/google/protobuf/wire_format_lite.cc
)

if (MSVC)
  set(protobuf_STAT_SRCS
    ${protobuf_STAT_SRCS}
    src/google/protobuf/stubs/atomicops_internals_x86_msvc.cc
  )
else()
  set(protobuf_STAT_SRCS
    ${protobuf_STAT_SRCS}
    src/google/protobuf/stubs/atomicops_internals_x86_gcc.cc
  )
endif()

add_library(protobuf ${protobuf_STAT_SRCS})

target_include_directories(protobuf
  PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src
  PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/sc_custom)

target_link_libraries(protobuf
  PRIVATE
    serenity-default-interface
    serenity-no-warning-interface
  PUBLIC
    threads)

if (BUILD_SHARED_LIBS)
  target_compile_definitions(protobuf
    PRIVATE
      -DLIBPROTOBUF_EXPORTS
      -DLIBPROTOC_EXPORTS
    PUBLIC
      -DPROTOBUF_USE_DLLS)
endif()

if (MSVC)
  target_compile_options(protobuf
    PRIVATE
      /wd4996)

  target_compile_definitions(protobuf
    PRIVATE
      -D_SCL_SECURE_NO_WARNINGS)
endif()

set_target_properties(protobuf
  PROPERTIES
    FOLDER
      "dep")

if (BUILD_SHARED_LIBS)
  if (UNIX)
    install(TARGETS protobuf
      LIBRARY
        DESTINATION lib)
  elseif (WIN32)
    install(TARGETS protobuf
      RUNTIME
        DESTINATION "${CMAKE_INSTALL_PREFIX}")
  endif()
endif()
