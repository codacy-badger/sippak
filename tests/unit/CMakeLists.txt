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

function (ADD_CMOCKA_TEST test_name test_src)
  add_executable(${test_name} ${test_src} ${ARGV})
  target_link_libraries(${test_name} ${CMOCKA_LIBRARIES} ${PJSIP_LIBRARIES} ${EXTRA_LIBS})
  add_test(unit_${test_name} ${test_name})
  if (USE_MEMTEST)
    add_test (${test_name}_valgrind valgrind
      --error-exitcode=1 --read-var-info=yes
      --leak-check=full --show-leak-kinds=all
      ./${test_name}
      )
  endif (USE_MEMTEST)
endfunction (ADD_CMOCKA_TEST)

# test CLI arguments
add_cmocka_test(test_getopts test_getopts.c
  ${CMAKE_SOURCE_DIR}/src/app/media_helper.c
  ${CMAKE_SOURCE_DIR}/src/app/getopts.c)

# use bogus resolv.conf for tests
add_cmocka_test(test_dns_helper test_dns_helper.c
  ${CMAKE_SOURCE_DIR}/src/app/media_helper.c
  ${CMAKE_SOURCE_DIR}/src/app/getopts.c
  ${CMAKE_SOURCE_DIR}/src/app/dns.c
  )
target_link_libraries (test_dns_helper resolv)

# test sip helper functions
add_cmocka_test(test_sip_helper test_sip_helper.c
  ${CMAKE_SOURCE_DIR}/src/app/media_helper.c
  ${CMAKE_SOURCE_DIR}/src/app/getopts.c
  ${CMAKE_SOURCE_DIR}/src/app/sip_helper.c
  )

# test media helper functions
add_definitions(-DPJMEDIA_HAS_SPEEX_CODEC
                -DPJMEDIA_HAS_ILBC_CODEC
                -DPJMEDIA_HAS_GSM_CODEC
                -DPJMEDIA_HAS_G711_CODEC
                -DPJMEDIA_HAS_G722_CODEC
                -DPJMEDIA_HAS_INTEL_IPP
                -DPJMEDIA_HAS_L16_CODEC
                -DPJMEDIA_HAS_OPENCORE_AMRNB_CODEC
                -DPJMEDIA_HAS_SILK_CODEC
                -DPJMEDIA_HAS_OPUS_CODEC
                -DPJMEDIA_HAS_BCG729
                -DSIPPAK_UNIT_TESTS
  )
add_cmocka_test(test_media_helper test_media_helper.c
  ${CMAKE_SOURCE_DIR}/src/app/media_helper.c
  )

