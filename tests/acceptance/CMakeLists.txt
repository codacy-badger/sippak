#
# sippak -- SIP command line utility.
# Copyright (C) 2018, Stas Kobzar <staskobzar@modulis.ca>
#
# This file is part of sippak.
#
# sippack is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# sippack is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with sippack.  If not, see <http://www.gnu.org/licenses/>.
#

set (EXECMD ${CMAKE_BINARY_DIR}/src/${PROJECT_NAME})

# test CLI opts and arguments
include(CMakeGetopts.cmake)

# test sipp scenarious
find_program (SIPP NAMES sipp)
if (SIPP)
  include (CMakeSIPPScenarios.cmake)
else (SIPP)
  message (WARNING "Application \"sipp\" not found. Will not run sipp scenarious.")
endif (SIPP)

