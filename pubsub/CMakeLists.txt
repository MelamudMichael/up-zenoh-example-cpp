# Copyright (c) 2024 General Motors GTO LLC
#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
# SPDX-FileType: SOURCE
# SPDX-FileCopyrightText: 2024 General Motors GTO LLC
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.20)
project(pubsub VERSION 0.1.0 LANGUAGES CXX)

find_package(spdlog REQUIRED)
find_package(up-client-zenoh-cpp QUIET)
find_package(up-cpp QUIET)

add_definitions(-DSPDLOG_FMT_EXTERNAL)

if(DEFINED ENV{GITHUB_CICD_ZENOH_PATH})
    set(ZENOH_LIB $ENV{GITHUB_CICD_ZENOH_PATH})
else()
    find_library(ZENOH_LIB zenohc)
endif() 

# sub
add_executable(sub src/main_sub.cpp)
target_link_libraries(sub
    PUBLIC
        ${ZENOH_LIB}
        up-client-zenoh-cpp::up-client-zenoh-cpp)

target_include_directories(sub 
    PUBLIC
        $ENV{CMAKE_ZENOH_INCLUDE_PATH}
        ${fmt_INCLUDE_DIR}
        ${spdlog_INCLUDE_DIR}
        ${up-cpp_INCLUDE_DIR})

# pub
add_executable(pub src/main_pub.cpp)
target_link_libraries(pub
    PUBLIC
        ${ZENOH_LIB}
        up-client-zenoh-cpp::up-client-zenoh-cpp)

target_include_directories(pub 
    PUBLIC
        $ENV{CMAKE_ZENOH_INCLUDE_PATH}
        ${fmt_INCLUDE_DIR}
        ${spdlog_INCLUDE_DIR}
        ${up-cpp_INCLUDE_DIR})
