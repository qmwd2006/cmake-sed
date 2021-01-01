cmake_minimum_required(VERSION 3.0)

project(test)

add_custom_command(
	OUTPUT utils.cpp
	COMMAND cat ${CMAKE_CURRENT_SOURCE_DIR}/private.pem
	| sed -ne "1d;\$d;s/.*/&\\\\n\\\\/;p"
	| sed -e "1i\\#include \"stdio.h\"\\n\\nconst char * info = \"\\\\"
	| sed -e "\$a\";\\n\\nvoid print_info()\\n{\\n    printf(info);\\n}\\n"
	> ${CMAKE_CURRENT_SOURCE_DIR}/utils.cpp
	MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/private.pem
	VERBATIM
)

add_executable(ftest main.cpp ${CMAKE_CURRENT_SOURCE_DIR}/utils.cpp)

