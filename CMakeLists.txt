cmake_minimum_required(VERSION 3.5)
project(all_any_none_program VERSION 0.1 LANGUAGES CXX )

# set the compiling flags
set ( CMAKE_CXX_FLAGS "-Wall" )

#--------------------------------
# This is for old cmake versions
#set (CMAKE_CXX_STANDARD 11)
#--------------------------------

### Application target
# configura o diretório contendo os .h, assim você pode inclui-los apenas com #include "arquivo.h"
include_directories( include )

# Cria um executável com nome "dnaprofiler" com base no main.cpp
set( APP_NAME "dnaprofiler" )
add_executable( ${APP_NAME}
                src/main.cpp )      # Inserir arquivos adicionais a partir desta linha, dentro da diretiva add_executable

# define C++11 standard
set_property(TARGET ${APP_NAME} PROPERTY CXX_STANDARD 11)

# The end
