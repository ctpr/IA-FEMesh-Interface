INCLUDE_REGULAR_EXPRESSION("^.*$")

INCLUDE_DIRECTORIES( ${IAFEMMESH_INCLUDE_DIR} )
INCLUDE_DIRECTORIES( ${IAFEMMESH_INCLUDE_DIR}/IaFeMesh )
INCLUDE_DIRECTORIES( ${IAFEMMESH_INCLUDE_DIR}/IaFeMesh/Mesh )
INCLUDE_DIRECTORIES( ${IAFEMMESH_INCLUDE_DIR}/IaFeMesh/IO )
INCLUDE_DIRECTORIES( ${IAFEMMESH_INCLUDE_DIR}/IaFeMesh/Adapter )
INCLUDE_DIRECTORIES( ${IAFEMMESH_INCLUDE_DIR}/IaFeMesh/Common )
INCLUDE_DIRECTORIES( ${IAFEMMESH_INCLUDE_DIR}/IaFeMesh/Filter )
INCLUDE_DIRECTORIES( ${IAFEMMESH_INCLUDE_DIR}/IaFeMesh/Widgets )


IF( NOT DISABLE_CXX_TESTING )
IF(ITK_BINARY_DIR)
  IF(TCL_TCLSH)
    ADD_TEST(PrintSelf-BasicFilters ${TCL_TCLSH}
             ${ITK_SOURCE_DIR}/Utilities/Dart/PrintSelfCheck.tcl  
             ${ITK_SOURCE_DIR}/Code/BasicFilters)
  ENDIF(TCL_TCLSH)
ENDIF(ITK_BINARY_DIR)



SET(TestPolygon3DSpatialObject ${CXX_TEST_PATH}/TestPolygon3DSpatialObject)
ADD_TEST(TestPolygon3DSpatialObject ${INPUTDATA}/BBox2.vtk)

SET(TestCurveToStructuredPlanarMesh ${CXX_TEST_PATH}/TestCurveToStructuredPlanarMesh)
ADD_TEST(TestCurveToStructuredPlanarMesh ${INPUTDATA}/BBox2.vtk)


ADD_EXECUTABLE(TestPolygon3DSpatialObject MainPolygon3DSpatialObject.cxx )
TARGET_LINK_LIBRARIES(TestPolygon3DSpatialObject iamAdapter ITKCommon ITKBasicFilters ITKIO vtkIO vtkHybrid vtkFiltering vtkCommon)

ADD_EXECUTABLE(TestCurveToStructuredPlanarMesh MainitkCurveToStructuredPlanarMesh.cxx )
TARGET_LINK_LIBRARIES(TestCurveToStructuredPlanarMesh iamAdapter ITKCommon ITKBasicFilters ITKIO itkvnl_algo itkvnl vtkIO vtkHybrid vtkFiltering vtkCommon)

ADD_EXECUTABLE(itkImageToVtkUnstructuredGridTest itkImageToVtkUnstructuredGridTest.cxx)
TARGET_LINK_LIBRARIES(itkImageToVtkUnstructuredGridTest iamFilter iamCommon 
                      vtkHybrid vtkRendering vtkGraphics vtkImaging vtkIO vtkFiltering vtkCommon )
ADD_TEST(itkImageToVtkUnstructuredGridTest ${EXECUTABLE_OUTPUT_PATH}/itkImageToVtkUnstructuredGridTest)

ENDIF( NOT DISABLE_CXX_TESTING )
