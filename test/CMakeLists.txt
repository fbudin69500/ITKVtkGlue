itk_module_test()

set(ITKVtkGlueTests
  itkVtkConnectedComponentImageFilterTest
  itkVtkMedianFilterTest.cxx
  itkImageToVTKImageFilterTest.cxx
  itkImageToVTKImageFilterRGBTest.cxx
  itkVTKImageToImageFilterTest.cxx
  itkMeshToVTKUnstructuredGridFilterTest.cxx
  QuickViewTest.cxx
  # ### runViewImage ###
  # No test generated. Use it for view images (2D or 3D) with:
  # ITKVtkGlueTestDriver runViewImage image_file
  # or with a string to change title window, and change size of window
  # ITKVtkGlueTestDriver runViewImage image_file "MyImage"
  runViewImage.cxx
  )

CreateTestDriver(ITKVtkGlue "${ITKVtkGlue-Test_LIBRARIES}" "${ITKVtkGlueTests}")
target_link_libraries_with_dynamic_lookup(ITKVtkGlueTestDriver PUBLIC ${ITKVtkGlue_VTK_LIBRARIES})

itk_add_test(NAME itkVtkMedianImageFilterTest
  COMMAND ITKVtkGlueTestDriver
    itkVtkMedianFilterTest DATA{Input/cthead1.png} 2
  )
set_property(TEST itkVtkMedianImageFilterTest APPEND PROPERTY LABELS REQUIRES_DISPLAY)

itk_add_test(NAME itkImageToVTKImageFilterTest
  COMMAND ITKVtkGlueTestDriver
    itkImageToVTKImageFilterTest
  )

itk_add_test(NAME itkImageToVTKImageFilterRGBTest
  COMMAND ITKVtkGlueTestDriver
    itkImageToVTKImageFilterRGBTest DATA{Input/VisibleWomanEyeSlice.png}
  )

itk_add_test(NAME itkVTKImageToImageFilterTest
  COMMAND ITKVtkGlueTestDriver
    itkVTKImageToImageFilterTest
  )

itk_add_test(NAME QuickViewTest
  COMMAND ITKVtkGlueTestDriver
           --compare DATA{Baseline/QuickViewTest.png,:}
                     ${ITK_TEST_OUTPUT_DIR}/QuickViewTest0.png
           --compareNumberOfPixelsTolerance 1500
    QuickViewTest DATA{Input/peppers.png} ${ITK_TEST_OUTPUT_DIR}
  )
set_property(TEST QuickViewTest APPEND PROPERTY LABELS REQUIRES_DISPLAY)

itk_add_test(NAME itkVtkConnectedComponentImageFilterTest
  COMMAND ITKVtkGlueTestDriver
    itkVtkConnectedComponentImageFilterTest DATA{Input/cthead1.png}
  )
set_property(TEST itkVtkConnectedComponentImageFilterTest APPEND PROPERTY LABELS REQUIRES_DISPLAY)

itk_add_test(NAME itkMeshToVTKUnstructuredGridFilterTest
  COMMAND ITKVtkGlueTestDriver
  itkMeshToVTKUnstructuredGridFilterTest DATA{Input/cow.vtk} ${ITK_TEST_OUTPUT_DIR}/cow_converted.vtk DATA{Input/cow.vtk}
  )
