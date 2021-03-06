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

CollectSourceFiles(
        ${CMAKE_CURRENT_SOURCE_DIR}
        PRIVATE_SOURCES
        # Exclude
        ${CMAKE_CURRENT_SOURCE_DIR}/PrecompiledHeaders)


if (USE_COREPCH)
    set(PRIVATE_PCH_HEADER PrecompiledHeaders/WorldserverPCH.hpp)
endif ()


GroupSources(${CMAKE_CURRENT_SOURCE_DIR})

add_executable(Worldserver
        ${PRIVATE_SOURCES})

set(WORLDSERVER_LINK_FLAGS "-pthread -lncurses ${WORLDSERVER_LINK_FLAGS}")
set_target_properties(Worldserver
        PROPERTIES
            LINK_FLAGS
                "${WORLDSERVER_LINK_FLAGS}")

target_link_libraries(Worldserver
        PRIVATE
            serenity-core-interface
        PUBLIC
            Framework
            Database
            readline)

CollectIncludeDirectories(
        ${CMAKE_CURRENT_SOURCE_DIR}
        PUBLIC_INCLUDES
        # Exclude
        ${CMAKE_CURRENT_SOURCE_DIR}/PrecompiledHeaders)

target_include_directories(Worldserver
        PUBLIC
            ${PUBLIC_INCLUDES}
        PRIVATE
        ${CMAKE_CURRENT_BINARY_DIR})

set_target_properties(Worldserver
        PROPERTIES
            FOLDER
                "Server")

if (USE_COREPCH)
    add_cxx_pch(Worldserver ${PRIVATE_PCH_HEADER})
endif ()