cmake_minimum_required(VERSION 3.25)

project(SpaceMonger VERSION 1.4.2 LANGUAGES CXX RC)

if(NOT MSVC)
  message(FATAL_ERROR "SpaceMonger is a Win32 MFC application and currently requires MSVC.")
endif()

if(NOT CMAKE_SIZEOF_VOID_P EQUAL 4)
  message(FATAL_ERROR "Configure with a 32-bit generator/platform, for example: cmake --preset vs2022-win32")
endif()

get_filename_component(_msvc_cl_dir "${CMAKE_CXX_COMPILER}" DIRECTORY)
get_filename_component(_msvc_host_dir "${_msvc_cl_dir}" DIRECTORY)
get_filename_component(_msvc_bin_dir "${_msvc_host_dir}" DIRECTORY)
get_filename_component(_msvc_tool_dir "${_msvc_bin_dir}" DIRECTORY)
if(NOT EXISTS "${_msvc_tool_dir}/ATLMFC/include/afxwin.h")
  message(FATAL_ERROR
    "MFC headers were not found for the selected MSVC toolset. "
    "Install the Visual Studio component 'C++ MFC for latest v143 build tools (x86 & x64)' "
    "and then rerun: cmake --preset vs2022-win32"
  )
endif()

enable_testing()

set(SPACEMONGER_SOURCES
  AboutDialog.cpp
  DriveDialog.cpp
  FolderTree.cpp
  FolderView.cpp
  Freedoc.cpp
  Freeview.cpp
  lang_en.cpp
  lang_fr.cpp
  Mainfrm.cpp
  SetupDlg.cpp
  SpaceMonger.cpp
  StdAfx.cpp
  TipWnd.cpp
  SpaceMonger.rc
  SpaceMonger.manifest
)

add_executable(SpaceMonger WIN32 ${SPACEMONGER_SOURCES})
target_include_directories(SpaceMonger PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_definitions(SpaceMonger PRIVATE WIN32 _WINDOWS _MBCS)
target_compile_options(SpaceMonger PRIVATE /W3 /EHsc)
target_precompile_headers(SpaceMonger PRIVATE Stdafx.h)
set_target_properties(SpaceMonger PROPERTIES
  MFC_FLAG 1
  MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>"
)


