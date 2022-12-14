cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
set(PIM_VERSION "5.18.1")

project(MailImporter VERSION ${PIM_VERSION})

set(KF5_MIN_VERSION "5.83.0")

find_package(ECM ${KF5_MIN_VERSION} CONFIG REQUIRED)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH})

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDEFrameworkCompilerSettings NO_POLICY_SCOPE)

include(GenerateExportHeader)
include(ECMSetupVersion)
include(ECMGenerateHeaders)
include(ECMGeneratePriFile)

include(FeatureSummary)
include(KDEGitCommitHooks)
include(ECMQtDeclareLoggingCategory)
include(ECMAddTests)



set(MAILIMPORTER_LIB ${PIM_VERSION})


set(AKONADIMIME_LIB_VERSION "5.18.1")
set(QT_REQUIRED_VERSION "5.15.0")
set(KMIME_LIB_VERSION "5.18.1")
set(AKONADI_VERSION "5.18.1")
set(PIMCOMMON_LIB_VERSION "5.18.1")

find_package(Qt5 ${QT_REQUIRED_VERSION} CONFIG REQUIRED Widgets Test Xml)

find_package(KF5KIO ${KF5_MIN_VERSION} CONFIG REQUIRED)
find_package(KF5Archive ${KF5_MIN_VERSION} CONFIG REQUIRED)
find_package(KF5Config ${KF5_MIN_VERSION} CONFIG REQUIRED)
find_package(KF5CoreAddons ${KF5_MIN_VERSION} CONFIG REQUIRED)
find_package(KF5I18n ${KF5_MIN_VERSION} CONFIG REQUIRED)

find_package(KF5Akonadi ${AKONADI_VERSION} CONFIG REQUIRED)
find_package(KF5Mime ${KMIME_LIB_VERSION} CONFIG REQUIRED)
find_package(KF5AkonadiMime ${AKONADIMIME_LIB_VERSION} CONFIG REQUIRED)
find_package(KF5PimCommonAkonadi ${PIMCOMMON_LIB_VERSION} CONFIG REQUIRED)
########### Targets ###########


add_definitions(-DQT_DISABLE_DEPRECATED_BEFORE=0x050f00)
add_definitions(-DKF_DISABLE_DEPRECATED_BEFORE_AND_AT=0x055400)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

########### CMake Config Files ###########

add_definitions(-DTRANSLATION_DOMAIN=\"libmailimporter\")

if(BUILD_TESTING)
   add_definitions(-DBUILD_TESTING)
endif()

option(USE_UNITY_CMAKE_SUPPORT "Use UNITY cmake support (speedup compile time)" OFF)

set(COMPILE_WITH_UNITY_CMAKE_SUPPORT OFF)
if (USE_UNITY_CMAKE_SUPPORT)
    set(COMPILE_WITH_UNITY_CMAKE_SUPPORT ON)
endif()

add_subdirectory(src)

ecm_qt_install_logging_categories(
        EXPORT MAILIMPORTER
        FILE mailimporter.categories
        DESTINATION ${KDE_INSTALL_LOGGINGCATEGORIESDIR}
        )

kde_configure_git_pre_commit_hook(CHECKS CLANG_FORMAT)
ki18n_install(po)
feature_summary(WHAT ALL FATAL_ON_MISSING_REQUIRED_PACKAGES)
