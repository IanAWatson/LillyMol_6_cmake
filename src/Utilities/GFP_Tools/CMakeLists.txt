project(gfp)

set(gfp_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/bic_table.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/dyfp.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/fixed_size_counted_fingerprint_uchar.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/_fixed_size_counted_fingerprint_uchar.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/_fixed_size_counted_fingerprint_uint.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/multi_conformer.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/sparsefp.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/sparse_collection.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/tversky.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/various_distance_metrics.cc
)

add_library(gfp ${gfp_srcs})
target_include_directories(gfp PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set(smiles_id_dist_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/smiles_id_dist.cc
)
add_library(smiles_id_dist ${smiles_id_dist_srcs})
target_include_directories(smiles_id_dist PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set(build_pool_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/build_pool.cc
)
add_library(build_pool ${build_pool_srcs})
target_include_directories(build_pool PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set(spread_v2_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/spread_v2.cc
)
add_library(spread_v2 ${spread_v2_srcs})
target_include_directories(spread_v2 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set(gfp_standard_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_standard.cc
)
add_library(gfp_standard ${gfp_standard_srcs})
target_include_directories(gfp_standard PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set(neighbour_list_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/neighbour_list.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/_neighbour_list_fp_and_smiles.cc
)
add_library(neighbour_list ${neighbour_list_srcs})
target_include_directories(neighbour_list PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set(metric_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/Enrichment.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/Metric.cc
)
add_library(metric ${metric_srcs})
target_include_directories(metric PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set(bsquared_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/bsquared.cc
)
add_library(bsquared ${bsquared_srcs})
target_include_directories(bsquared PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set(common_libs
  accumulator
  cmdline
  iwstring_data_source
  iwre2
  gfp
  sparse_fp_creator
  iwbits
  iw_tdt
  iwstring
  iwmisc
  ${IW_EXTRA_LIBS}
)

# descriptor_file_to_01_fingerprints
set(descriptor_file_to_01_fingerprints_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/descriptor_file_to_01_fingerprints.cc
)
set(descriptor_file_to_01_fingerprints_libs
  ${common_libs}
)
add_executable(descriptor_file_to_01_fingerprints ${descriptor_file_to_01_fingerprints_srcs})
target_link_libraries(descriptor_file_to_01_fingerprints LINK_PUBLIC ${descriptor_file_to_01_fingerprints_libs})

# gfp_spread
set(gfp_spread_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_spread_v2.cc
)
set(gfp_spread_libs
  spread_v2
  smiles_id_dist
  ${common_libs}
)
add_executable(gfp_spread ${gfp_spread_srcs})
target_link_libraries(gfp_spread LINK_PUBLIC ${gfp_spread_libs})


set(gfp_leader_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_leader_v2.cc
)
set(gfp_leader_libs
  ${common_libs}
  smiles_id_dist
)
add_executable(gfp_leader ${gfp_leader_srcs})
target_link_libraries(gfp_leader LINK_PUBLIC ${gfp_leader_libs})


set(gfp_leader_standard_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_leader_standard.cc
)
set(gfp_leader_standard_libs
  ${common_libs}
  smiles_id_dist
  gfp_standard
)
add_executable(gfp_leader_standard ${gfp_leader_standard_srcs})
target_link_libraries(gfp_leader_standard LINK_PUBLIC ${gfp_leader_standard_libs} -fopenmp)


set(gfp_naive_bayesian_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_naive_bayesian.cc
)
set(gfp_naive_bayesian_libs
  ${common_libs}
)
add_executable(gfp_naive_bayesian ${gfp_naive_bayesian_srcs})
target_link_libraries(gfp_naive_bayesian LINK_PUBLIC ${gfp_naive_bayesian_libs} -fopenmp)


set(gfp_spread_standard_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_spread_standard.cc
)
set(gfp_spread_standard_libs
  ${common_libs}
  gfp_standard
)
add_executable(gfp_spread_standard ${gfp_spread_standard_srcs})
target_link_libraries(gfp_spread_standard LINK_PUBLIC ${gfp_spread_standard_libs} -fopenmp)


set(gfp_nearneighbours_single_file_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_nearneighbours_single_file.cc
)
set(gfp_nearneighbours_single_file_libs
  ${common_libs}
  gfp_standard
  iwhistogram
)
add_executable(gfp_nearneighbours_single_file ${gfp_nearneighbours_single_file_srcs})
target_link_libraries(gfp_nearneighbours_single_file LINK_PUBLIC ${gfp_nearneighbours_single_file_libs} -fopenmp)


# gfp_lnearneighbours
set(gfp_lnearneighbours_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_lnearneighbours.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/fp_and_smiles.cc
)
set(gfp_lnearneighbours_libs
  neighbour_list
  smiles_id_dist
  ${common_libs}
)
add_executable(gfp_lnearneighbours ${gfp_lnearneighbours_srcs})
target_link_libraries(gfp_lnearneighbours LINK_PUBLIC ${gfp_lnearneighbours_libs})

# gfp_lnearneighbours_standard
set(gfp_lnearneighbours_standard_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_lnearneighbours_standard.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/fp_and_smiles.cc
)
set(gfp_lnearneighbours_standard_libs
  accumulator
  neighbour_list
  smiles_id_dist
  gfp_standard
  ${common_libs}
  -lgomp
)
add_executable(gfp_lnearneighbours_standard ${gfp_lnearneighbours_standard_srcs})
target_link_libraries(gfp_lnearneighbours_standard LINK_PUBLIC ${gfp_lnearneighbours_standard_libs})



set(iwstats_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/iwstats.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/fp_and_smiles.cc
)
set(iwstats_libs
  metric
  bsquared
  ${common_libs}
)
add_executable(iwstats ${iwstats_srcs})
target_link_libraries(iwstats LINK_PUBLIC ${iwstats_libs})

# gfp_profile_activity_by_bits
set(gfp_profile_activity_by_bits_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_profile_activity_by_bits.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/fp_and_smiles.cc
)
set(gfp_profile_activity_by_bits_libs
  neighbour_list
  smiles_id_dist
  ${common_libs}
)
add_executable(gfp_profile_activity_by_bits ${gfp_profile_activity_by_bits_srcs})
target_link_libraries(gfp_profile_activity_by_bits LINK_PUBLIC ${gfp_profile_activity_by_bits_libs})

# gfp_to_descriptors
set(gfp_to_descriptors_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_to_descriptors.cc
)
set(gfp_to_descriptors_libs
  ${common_libs}
)
add_executable(gfp_to_descriptors ${gfp_to_descriptors_srcs})
target_link_libraries(gfp_to_descriptors LINK_PUBLIC ${gfp_to_descriptors_libs})

# nplotnn
set(nplotnn_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/nplotnn.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/distance_scaling.cc
)
set(nplotnn_libs
  smiles_id_dist
  moleculeio
  iwmolecule
  iwhistogram
  string_data_source
  xmlParser
  ${common_libs}
)
add_executable(nplotnn ${nplotnn_srcs})
target_link_libraries(nplotnn LINK_PUBLIC ${nplotnn_libs})

# gfp_distance_matrix
set(gfp_distance_matrix_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_distance_matrix.cc
)
set(gfp_distance_matrix_libs
  build_pool
  iwhistogram
  ${common_libs}
)
add_executable(gfp_distance_matrix ${gfp_distance_matrix_srcs})
target_link_libraries(gfp_distance_matrix LINK_PUBLIC ${gfp_distance_matrix_libs})

# tdt_sort
set(tdt_sort_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/tdt_sort.cc
)
set(tdt_sort_libs
  cmdline_v2
  iwstring_data_source
  iwre2
  iw_tdt
  iwstring
  iwmisc
  -lre2 -lz
)
add_executable(tdt_sort ${tdt_sort_srcs})
target_link_libraries(tdt_sort LINK_PUBLIC ${tdt_sort_libs})

# gfp_distance_filter
set(gfp_distance_filter_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_distance_filter.cc
)
set(gfp_distance_filter_libs
  cmdline_v2
  iwstring_data_source
  iwre2
  iw_tdt
  iwstring
  iwmisc
  ${common_libs}
)
add_executable(gfp_distance_filter ${gfp_distance_filter_srcs})
target_link_libraries(gfp_distance_filter LINK_PUBLIC ${gfp_distance_filter_libs})

# nn_leader_and_jp
set(nn_leader_and_jp_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/nn_leader_and_jp.cc
)
set(nn_leader_and_jp_libs
  cmdline_v2
  iwstring_data_source
  iwre2
  iw_tdt
  iwstring
  iwmisc
  ${common_libs}
)
add_executable(nn_leader_and_jp ${nn_leader_and_jp_srcs})
target_link_libraries(nn_leader_and_jp LINK_PUBLIC ${nn_leader_and_jp_libs})

# gfp_pairwise_distances
set(gfp_pairwise_distances_srcs
  ${CMAKE_CURRENT_SOURCE_DIR}/gfp_pairwise_distances.cc
)
set(gfp_pairwise_distances_libs
  cmdline_v2
  iwhistogram
  iwstring_data_source
  iwre2
  iw_tdt
  iwstring
  iwmisc
  ${common_libs}
)
add_executable(gfp_pairwise_distances ${gfp_pairwise_distances_srcs})
target_link_libraries(gfp_pairwise_distances LINK_PUBLIC ${gfp_pairwise_distances_libs})
