cmake_minimum_required(VERSION 3.5)

set(KF5_VERSION "5.57.0") # handled by release scripts
set(KF5_DEP_VERSION "5.70.0") # handled by release scripts

project(kirigami-addons VERSION ${KF5_VERSION})

set(REQUIRED_QT_VERSION 5.15.0)

SET(CMAKE_CXX_STANDARD 11)

# Make CPack available to easy generate binary packages
include(CPack)
include(FeatureSummary)

find_package(Qt5 ${REQUIRED_QT_VERSION} REQUIRED NO_MODULE COMPONENTS Core Quick QuickControls2)

set(CMAKE_AUTOMOC ON)
set(AUTOMOC_MOC_OPTIONS -Muri=org.kde.kirigami)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

################# set KDE specific information #################

find_package(ECM 5.56.0 NO_MODULE)
set_package_properties(ECM PROPERTIES TYPE REQUIRED DESCRIPTION "Extra CMake Modules." URL "https://projects.kde.org/projects/kdesupport/extra-cmake-modules")

# where to look first for cmake modules, before ${CMAKE_ROOT}/Modules/ is checked
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

include(GenerateExportHeader)
include(ECMSetupVersion)
include(ECMGenerateHeaders)
include(CMakePackageConfigHelpers)
include(ECMPoQmTools)
include(ECMQMLModules)
include(KDEInstallDirs)
include(KDECMakeSettings)
include(ECMQtDeclareLoggingCategory)
include(ECMAddQch)
include(KDEFrameworkCompilerSettings NO_POLICY_SCOPE)

find_package(KF5 ${KF5_DEP_VERSION} REQUIRED COMPONENTS Kirigami2 I18n)

set(CMAKECONFIG_INSTALL_DIR "${KDE_INSTALL_CMAKEPACKAGEDIR}/KF5KirigamiAddons")

option(BUILD_QCH "Build API documentation in QCH format (for e.g. Qt Assistant, Qt Creator & KDevelop)" OFF)
add_feature_info(QCH ${BUILD_QCH} "API documentation in QCH format (for e.g. Qt Assistant, Qt Creator & KDevelop)")

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/KF5KirigamiAddonsConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/KF5KirigamiAddonsConfig.cmake"
    INSTALL_DESTINATION  ${CMAKECONFIG_INSTALL_DIR}
    PATH_VARS  KF5_INCLUDE_INSTALL_DIR CMAKE_INSTALL_PREFIX
)

# install(FILES
#     "${CMAKE_CURRENT_BINARY_DIR}/KF5KirigamiAddonsConfig.cmake"
#     "${CMAKE_CURRENT_BINARY_DIR}/KF5KirigamiAddonsConfigVersion.cmake"
#     "${CMAKE_CURRENT_SOURCE_DIR}/KF5KirigamiAddonsMacros.cmake"
#     DESTINATION "${CMAKECONFIG_INSTALL_DIR}"
#     COMPONENT Devel
# )

# install(EXPORT KF5KirigamiAddonsTargets
#     DESTINATION "${CMAKECONFIG_INSTALL_DIR}"
#     FILE KF5KirigamiAddonsTargets.cmake
#     NAMESPACE KF5::
# )

# ecm_setup_version(${KF5_VERSION}
#     VARIABLE_PREFIX KIRIGAMIADDONS
#     VERSION_HEADER "${CMAKE_CURRENT_BINARY_DIR}/kirigamiaddons_version.h"
#     PACKAGE_VERSION_FILE "${CMAKE_CURRENT_BINARY_DIR}/KF5KirigamiAddonsConfigVersion.cmake"
#     SOVERSION 5
# )

add_subdirectory(src)

feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)

