# Copyright (c) 2026 Brandon Edmonds
# SPDX-License-Identifier: Apache-2.0

zephyr_include_directories(include)

add_subdirectory_ifdef(CONFIG_ICM20948 drivers/icm20948)
