# This is the root IA-FEMesh-Interface CMakeLists file.
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)


PROJECT( IA-FEMesh-Interface )

# On Visual Studio 8 MS deprecated C. This removes all 1.276E1265 security
# warnings
IF(WIN32)
  IF(NOT BORLAND)
    IF(NOT CYGWIN)
      IF(NOT MINGW)
        IF(NOT ITK_ENABLE_VISUAL_STUDIO_DEPRECATED_C_WARNINGS)
          ADD_DEFINITIONS(
            -D_CRT_FAR_MAPPINGS_NO_DEPRECATE
            -D_CRT_IS_WCTYPE_NO_DEPRECATE
            -D_CRT_MANAGED_FP_NO_DEPRECATE
            -D_CRT_NONSTDC_NO_DEPRECATE
            -D_CRT_SECURE_NO_DEPRECATE
            -D_CRT_SECURE_NO_DEPRECATE_GLOBALS
            -D_CRT_SETERRORMODE_BEEP_SLEEP_NO_DEPRECATE
            -D_CRT_TIME_FUNCTIONS_NO_DEPRECATE
            -D_CRT_VCCLRIT_NO_DEPRECATE
            -D_SCL_SECURE_NO_DEPRECATE
            )
        ENDIF(NOT ITK_ENABLE_VISUAL_STUDIO_DEPRECATED_C_WARNINGS)
      ENDIF(NOT MINGW)
    ENDIF(NOT CYGWIN)
  ENDIF(NOT BORLAND)
ENDIF(WIN32)


# If CMake is the version we're expecting, don't show the
# CMAKE_BACKWARDS_COMPATIBILITY option. If the version is higher than the
# minimal version required, then show the backward compatibility option.
IF("${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}" MATCHES "^2\\.4$")
  MARK_AS_ADVANCED(FORCE CMAKE_BACKWARDS_COMPATIBILITY)
ELSE("${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}" MATCHES "^2\\.4$")
  MARK_AS_ADVANCED(CLEAR CMAKE_BACKWARDS_COMPATIBILITY)
ENDIF("${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}" MATCHES "^2\\.4$")

#-----------------------------------------------------------------------------
# IA-FEMesh-Interface version number.  An even minor number corresponds to releases.
SET(IA-FEMesh-Interface_VERSION_MAJOR "1")
SET(IA-FEMesh-Interface_VERSION_MINOR "0")
SET(IA-FEMesh-Interface_VERSION_PATCH "0")

# Version string should not include patch level.  The major.minor is
# enough to distinguish available features of the toolkit.
SET(IA-FEMesh-Interface_VERSION_STRING "${IA-FEMesh-Interface_VERSION_MAJOR}.${IA-FEMesh-Interface_VERSION_MINOR}.${IA-FEMesh-Interface_VERSION_PATCH}")

#-----------------------------------------------------------------------------
# Test for some required system information.
INCLUDE (${CMAKE_ROOT}/Modules/CMakeBackwardCompatibilityC.cmake)
INCLUDE (${CMAKE_ROOT}/Modules/CMakeBackwardCompatibilityCXX.cmake)

#--------------------------
# Find IA-FEMesh API
FIND_PATH(IAFEMMESH_INCLUDE_DIR NAMES IaFeMesh/Common/vtkMimxActorBase.h)
FIND_PATH(IAFEMMESH_LIBRARY_DIR NAMES IaFeMesh/libmimxCommon.a)
FIND_PATH(IAFEMMESH_BINARY_DIR NAMES IA-FEMesh)

#--------------------------

#IF(WIN32)
#  LINK_LIBRARIES( wsock32 )
#ENDIF(WIN32)

INCLUDE (${CMAKE_ROOT}/Modules/FindITK.cmake)
FIND_PACKAGE(ITK)
IF(ITK_FOUND)
  INCLUDE (${USE_ITK_FILE})
  LINK_LIBRARIES( ITKAlgorithms ITKIO ITKBasicFilters ITKCommon itkvnl_algo itkvnl )
  LINK_LIBRARIES ( itkzlib itksys )
ENDIF(ITK_FOUND)

INCLUDE (${CMAKE_ROOT}/Modules/FindVTK.cmake)
FIND_PACKAGE(VTK)
IF(VTK_FOUND)
  INCLUDE (${USE_VTK_FILE})
  LINK_LIBRARIES( vtkHybrid vtkWidgets vtkRendering  vtkGraphics vtkImaging vtkIO vtkFiltering vtkCommon )
  IF (${VTK_MAJOR_VERSION} GREATER 4)
    LINK_LIBRARIES( vtkWidgets )
  ENDIF (${VTK_MAJOR_VERSION} GREATER 4)
ENDIF(VTK_FOUND)

#
## KWWidgets
#
#FIND_PACKAGE(KWWidgets REQUIRED)
#IF (NOT KWWidgets_FOUND)
#  MESSAGE (FATAL_ERROR "Need KWWidgets to build")
#ENDIF (NOT KWWidgets_FOUND)
#INCLUDE(${KWWidgets_USE_FILE})

#-----------------------------------------------------------------------------
# Output directories.
IF(NOT LIBRARY_OUTPUT_PATH)
  SET (LIBRARY_OUTPUT_PATH ${IA-FEMesh-Interface_BINARY_DIR}/lib CACHE INTERNAL "Single output directory for building all libraries.")
ENDIF(NOT LIBRARY_OUTPUT_PATH)
IF(NOT EXECUTABLE_OUTPUT_PATH)
  SET (EXECUTABLE_OUTPUT_PATH ${IA-FEMesh-Interface_BINARY_DIR}/bin CACHE INTERNAL "Single output directory for building all executables.")
ENDIF(NOT EXECUTABLE_OUTPUT_PATH)
MARK_AS_ADVANCED(LIBRARY_OUTPUT_PATH EXECUTABLE_OUTPUT_PATH)
SET(IA-FEMesh-Interface_LIBRARY_PATH "${LIBRARY_OUTPUT_PATH}")
SET(IA-FEMesh-Interface_EXECUTABLE_PATH "${EXECUTABLE_OUTPUT_PATH}")
SET(CXX_TEST_PATH ${EXECUTABLE_OUTPUT_PATH})

#SET(IA-FEMesh-Interface_DATA_ROOT ${IA-FEMesh-Interface_SOURCE_DIR}/Testing/Data)

ENABLE_TESTING()


#-----------------------------------------------------------------------------
INCLUDE(InstallRequiredSystemLibraries)


SET(CPACK_PACKAGE_DESCRIPTION_SUMMARY "IA-FEMesh-InterfaceMesh")
SET(CPACK_PACKAGE_VENDOR "MIMX Program, Center of Computer Aided Design, The University of Iowa")
SET(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/ReadMe.txt")
SET(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/Copyright.txt")
SET(CPACK_PACKAGE_VERSION_MAJOR "1")
SET(CPACK_PACKAGE_VERSION_MINOR "0")
SET(CPACK_PACKAGE_VERSION_PATCH "0")
SET(CPACK_PACKAGE_INSTALL_DIRECTORY "IA-FEMesh-Interface ${IA-FEMesh-Interface_VERSION_MAJOR}.${IA-FEMesh-Interface_VERSION_MINOR}")
SET(CPACK_PACKAGE_FILE_NAME "IA-FEMesh-${IA-FEMesh-Interface_VERSION_MAJOR}.${IA-FEMesh-Interface_VERSION_MINOR}.${IA-FEMesh-Interface_VERSION_PATCH}")
IF(WIN32 AND NOT UNIX)
  # There is a bug in NSI that does not handle full unix paths properly. Make
  # sure there is at least one set of four (4) backlasshes.
  SET(CPACK_PACKAGE_ICON "${IA-FEMesh-Interface_SOURCE_DIR}\\\\Icon32.bmp")
  SET(CPACK_NSIS_INSTALLED_ICON_NAME "bin\\\\IA-FEMesh-Interface.exe")
  SET(CPACK_NSIS_DISPLAY_NAME "${CPACK_PACKAGE_INSTALL_DIRECTORY} IA-FEMesh-Interface")
  SET(CPACK_NSIS_HELP_LINK "http:\\\\\\\\www.ccad.uiowa.edu/mimx")
  SET(CPACK_NSIS_URL_INFO_ABOUT "http:\\\\\\\\www.ccad.uiowa.edu/mimx")
  SET(CPACK_NSIS_CONTACT "grosland@engineering.uiowa.edu")
  SET(CPACK_NSIS_MODIFY_PATH ON)
  SET(CPACK_NSIS_MUI_ICON "${IA-FEMesh-Interface_SOURCE_DIR}\\\\Icon32.ico")
ELSE(WIN32 AND NOT UNIX)
  SET(CPACK_STRIP_FILES "bin/IA-FEMesh-Interface")
  SET(CPACK_SOURCE_STRIP_FILES "")
ENDIF(WIN32 AND NOT UNIX)

SET(CPACK_PACKAGE_EXECUTABLES "IA-FEMesh-Interface")
#SET(CPACK_CREATE_DESKTOP_LINKS "IA-FEMesh-Interface")
INCLUDE(CPack)

#-----------------------------------------------------------------------------
SUBDIRS(Adapter Filter Mesh)

IF(WIN32)
  IF(NOT BORLAND)
    IF(NOT CYGWIN)
      IF(NOT MINGW)
        SUBDIRS(Windows)
      ENDIF(NOT MINGW)
    ENDIF(NOT CYGWIN)
  ENDIF(NOT BORLAND)
ENDIF(WIN32)


INSTALL_FILES(/bin ReadMe.txt)
INSTALL_FILES(/bin Copyright.txt)
CONFIGURE_FILE(${IA-FEMesh-Interface_SOURCE_DIR}/ReadMe.txt ${EXECUTABLE_OUTPUT_PATH}/ReadMe.txt COPYONLY IMMEDIATE)
CONFIGURE_FILE(${IA-FEMesh-Interface_SOURCE_DIR}/Copyright.txt ${EXECUTABLE_OUTPUT_PATH}/Copyright.txt COPYONLY IMMEDIATE)

