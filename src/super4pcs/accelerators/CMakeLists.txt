project (OpenGR-AccelLib)

set(accel_ROOT "super4pcs/accelerators")

#############################################

set(accel_relative_INCLUDE
    ${accel_ROOT}/kdtree.h
    ${accel_ROOT}/pairExtraction/bruteForceFunctor.h
    ${accel_ROOT}/pairExtraction/intersectionFunctor.h
    ${accel_ROOT}/pairExtraction/intersectionNode.h
    ${accel_ROOT}/pairExtraction/intersectionPrimitive.h
    ${accel_ROOT}/normalset.h
    ${accel_ROOT}/normalset.hpp
    ${accel_ROOT}/bbox.h
    ${accel_ROOT}/utils.h)


if(OpenGR_USE_CHEALPIX)
    include_directories(${Chealpix_INCLUDE_DIR})
    set(APPEND accel_relative_INCLUDE ${accel_ROOT}/normalHealSet.h)
endif(OpenGR_USE_CHEALPIX)

# compute accel_INCLUDE to get absolute filenames
set(accel_INCLUDE "")
set(accel_installed_INCLUDE "")
foreach(f ${accel_relative_INCLUDE})
    list(APPEND accel_INCLUDE "${SRC_DIR}/${f}")
    list(APPEND accel_installed_INCLUDE "include/${f}")
endforeach(f)

add_library(opengr_accel INTERFACE)
target_sources(opengr_accel INTERFACE
    $<BUILD_INTERFACE:${accel_INCLUDE}>
    $<INSTALL_INTERFACE:${accel_installed_INCLUDE}>
    )


if(OpenGR_USE_CHEALPIX)
    add_dependencies(opengr_accel chealpix)
endif(OpenGR_USE_CHEALPIX)

set(targets_export_name "${PROJECT_NAME}Targets")
install( TARGETS opengr_accel
    EXPORT "${targets_export_name}"
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    INCLUDES DESTINATION include  )
