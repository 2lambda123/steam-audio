# Copyright 2017 Valve Corporation. All rights reserved. Subject to the following license:
# https://valvesoftware.github.io/steam-audio/license.html

#
# VERSION STAMPING
#

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/SteamAudio.uplugin.in ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/SteamAudio.uplugin)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin.in ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin)

#
# UNREAL PLUGIN
#

set(UBT_ARGS "-iwyu -noubtmakefiles -nohotreload")

file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/HostProject.uproject "{ \"FileVersion\": 3, \"Plugins\": [ { \"Name\": \"SteamAudio\", \"Enabled\": true } ] }")

add_custom_target(phonon_unreal_setup
    COMMAND     cmake -E rm -rf ${CMAKE_CURRENT_BINARY_DIR}/HostProject
    COMMAND     cmake -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/HostProject
    COMMAND     cmake -E copy ${CMAKE_CURRENT_BINARY_DIR}/HostProject.uproject ${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject
    COMMAND     cmake -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio
    COMMAND     cmake -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio
    COMMAND     cmake -E rm -rf ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/Intermediate
    VERBATIM
)

add_custom_target(phonon_unreal_win32
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Win32 Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Win32-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Win32 Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Win32-Shipping.xml ${UBT_ARGS}
    VERBATIM
)

add_custom_target(phonon_unreal_win64
    COMMAND     ${Unreal_EXECUTABLE} UE4Editor Win64 Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Editor-Win64-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Win64 Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Win64-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Win64 Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Win64-Shipping.xml ${UBT_ARGS}
    VERBATIM
)

add_custom_target(phonon_unreal_linux
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Linux Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Linux-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Linux Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Linux-Shipping.xml ${UBT_ARGS}
    VERBATIM
)

add_custom_target(phonon_unreal_mac
    COMMAND     ${Unreal_EXECUTABLE} UE4Editor Mac Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Editor-Mac-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Mac Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Mac-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Mac Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Mac-Shipping.xml ${UBT_ARGS}
    VERBATIM
)

add_custom_target(phonon_unreal_android
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Android Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Android-Development.xml ${UBT_ARGS} -architectures=armv7+arm64
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Android Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProject/HostProject.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins/SteamAudio/SteamAudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Android-Shipping.xml ${UBT_ARGS} -architectures=armv7+arm64
    VERBATIM
)

add_custom_target(phonon_unreal ALL
    COMMAND     cmake -E echo Built plugin SteamAudio.
    VERBATIM
)

add_dependencies(phonon_unreal_win32 phonon_unreal_setup)
add_dependencies(phonon_unreal_win64 phonon_unreal_setup)
add_dependencies(phonon_unreal_linux phonon_unreal_setup)
add_dependencies(phonon_unreal_mac phonon_unreal_setup)
add_dependencies(phonon_unreal_android phonon_unreal_setup)

if (IPL_OS_WINDOWS AND IPL_CPU_X64)
    add_dependencies(phonon_unreal phonon_unreal_win64)
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_WIN32)
        add_dependencies(phonon_unreal phonon_unreal_win32)
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_LINUX)
        add_dependencies(phonon_unreal phonon_unreal_linux)
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_ANDROID)
        add_dependencies(phonon_unreal phonon_unreal_android)
    endif()
elseif (IPL_OS_MACOS)
    add_dependencies(phonon_unreal phonon_unreal_mac)
endif()

#
# UNREAL + FMOD STUDIO PLUGIN
#

file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD.uproject "{ \"FileVersion\": 3, \"Plugins\": [ { \"Name\": \"SteamAudio\", \"Enabled\": true }, { \"Name\": \"FMODStudio\", \"Enabled\": true }, { \"Name\": \"SteamAudioFMODStudio\", \"Enabled\": true } ] }")

add_custom_target(phonon_unreal_fmod_setup
    COMMAND     cmake -E rm -rf ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD
    COMMAND     cmake -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD
    COMMAND     cmake -E copy ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD.uproject ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject
    COMMAND     cmake -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudio
    COMMAND     cmake -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/FMODStudio
    COMMAND     cmake -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio
    COMMAND     cmake -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudio
    COMMAND     cmake -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/FMODStudio ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/FMODStudio
    COMMAND     cmake -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio
    COMMAND     cmake -E rm -rf ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/Intermediate
    COMMAND     cmake -E rm -f ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/FMODStudio/Binaries/Win64/UE4Editor.modules
    COMMAND     cmake -E rm -f ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/FMODStudio/Binaries/Win64/UE4Editor-FMODStudio.dll
    COMMAND     cmake -E rm -f ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/FMODStudio/Binaries/Win64/UE4Editor-FMODStudio.pdb
    COMMAND     cmake -E rm -f ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/FMODStudio/Binaries/Win64/UE4Editor-FMODStudioEditor.dll
    COMMAND     cmake -E rm -f ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/FMODStudio/Binaries/Win64/UE4Editor-FMODStudioEditor.pdb
    VERBATIM
)

add_custom_target(phonon_unreal_fmod_win32
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Win32 Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Win32-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Win32 Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Win32-Shipping.xml ${UBT_ARGS}
    VERBATIM
)

add_custom_target(phonon_unreal_fmod_win64
    COMMAND     ${Unreal_EXECUTABLE} UE4Editor Win64 Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Editor-Win64-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Win64 Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Win64-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Win64 Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Win64-Shipping.xml ${UBT_ARGS}
    VERBATIM
)

add_custom_target(phonon_unreal_fmod_linux
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Linux Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Linux-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Linux Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Linux-Shipping.xml ${UBT_ARGS}
    VERBATIM
)

add_custom_target(phonon_unreal_fmod_mac
    COMMAND     ${Unreal_EXECUTABLE} UE4Editor Mac Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Editor-Mac-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Mac Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Mac-Development.xml ${UBT_ARGS}
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Mac Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Mac-Shipping.xml ${UBT_ARGS}
    VERBATIM
)

add_custom_target(phonon_unreal_fmod_android
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Android Development -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Android-Development.xml ${UBT_ARGS} -architectures=armv7+arm64
    COMMAND     ${Unreal_EXECUTABLE} UE4Game Android Shipping -project=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/HostProjectFMOD.uproject -plugin=${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin -manifest=${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Android-Shipping.xml ${UBT_ARGS} -architectures=armv7+arm64
    VERBATIM
)

add_custom_target(phonon_unreal_fmod ALL
    COMMAND     cmake -E echo Built plugin SteamAudioFMODStudio.
    VERBATIM
)

add_dependencies(phonon_unreal_fmod_win32 phonon_unreal_fmod_setup)
add_dependencies(phonon_unreal_fmod_win64 phonon_unreal_fmod_setup)
add_dependencies(phonon_unreal_fmod_linux phonon_unreal_fmod_setup)
add_dependencies(phonon_unreal_fmod_mac phonon_unreal_fmod_setup)
add_dependencies(phonon_unreal_fmod_android phonon_unreal_fmod_setup)

if (IPL_OS_WINDOWS AND IPL_CPU_X64)
    add_dependencies(phonon_unreal_fmod phonon_unreal_fmod_win64)
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_WIN32)
        add_dependencies(phonon_unreal_fmod phonon_unreal_fmod_win32)
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_LINUX)
        add_dependencies(phonon_unreal_fmod phonon_unreal_fmod_linux)
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_ANDROID)
        add_dependencies(phonon_unreal_fmod phonon_unreal_fmod_android)
    endif()
elseif (IPL_OS_MACOS)
    add_dependencies(phonon_unreal_fmod phonon_unreal_fmod_mac)
endif()

add_dependencies(phonon_unreal_fmod phonon_unreal)

#
# INSTALL
#

if (IPL_OS_WINDOWS AND IPL_CPU_X64)
    install(CODE "execute_process(COMMAND cmake -E rm -r ${CMAKE_HOME_DIRECTORY}/bin/unreal/SteamAudio COMMAND_ECHO STDOUT)")
    install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/SteamAudio.uplugin DESTINATION unreal/SteamAudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/Source DESTINATION unreal/SteamAudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/Content DESTINATION unreal/SteamAudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/Resources DESTINATION unreal/SteamAudio)
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Editor-Win64-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Win64-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Win64-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_WIN32)
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Win32-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Win32-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_LINUX)
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Linux-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Linux-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_ANDROID)
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Android-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Android-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    endif()

    install(CODE "execute_process(COMMAND cmake -E rm -rf ${CMAKE_HOME_DIRECTORY}/bin/unreal/FMODStudio)")
    install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/FMODStudio/Binaries/Win64/phonon_fmod.dll DESTINATION unreal/FMODStudio/Binaries/Win64)
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_WIN32)
        install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/FMODStudio/Binaries/Win32/phonon_fmod.dll DESTINATION unreal/FMODStudio/Binaries/Win32)
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_LINUX)
        install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/FMODStudio/Binaries/Linux/libphonon_fmod.so DESTINATION unreal/FMODStudio/Binaries/Linux)
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_ANDROID)
        install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/FMODStudio/Binaries/Android/armeabi-v7a/libphonon_fmod.so DESTINATION unreal/FMODStudio/Binaries/Android/armeabi-v7a)
        install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/FMODStudio/Binaries/Android/arm64-v8a/libphonon_fmod.so DESTINATION unreal/FMODStudio/Binaries/Android/arm64-v8a)
        install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/FMODStudio/Binaries/Android/x86/libphonon_fmod.so DESTINATION unreal/FMODStudio/Binaries/Android/x86)
    endif()

    install(CODE "execute_process(COMMAND cmake -E rm -rf ${CMAKE_HOME_DIRECTORY}/bin/unreal/SteamAudioFMODStudio)")
    install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin DESTINATION unreal/SteamAudioFMODStudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/Source DESTINATION unreal/SteamAudioFMODStudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/Resources DESTINATION unreal/SteamAudioFMODStudio)
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Editor-Win64-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Win64-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Win64-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_WIN32)
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Win32-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Win32-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_LINUX)
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Linux-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Linux-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    endif()
    if (STEAMAUDIOUNREAL_CROSSCOMPILE_ANDROID)
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Android-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
        install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Android-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    endif()
elseif (IPL_OS_MACOS)
    install(CODE "execute_process(COMMAND cmake -E rm -rf ${CMAKE_HOME_DIRECTORY}/bin/unreal/SteamAudio)")
    install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/SteamAudio.uplugin DESTINATION unreal/SteamAudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/Source DESTINATION unreal/SteamAudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/Content DESTINATION unreal/SteamAudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudio/Resources DESTINATION unreal/SteamAudio)
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Editor-Mac-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Mac-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProject/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudio-UE4Game-Mac-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")

    install(CODE "execute_process(COMMAND cmake -E rm -rf ${CMAKE_HOME_DIRECTORY}/bin/unreal/FMODStudio)")
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/FMODStudio/Binaries/Mac/phonon_fmod.bundle DESTINATION unreal/FMODStudio/Binaries/Mac)

    install(CODE "execute_process(COMMAND cmake -E rm -rf ${CMAKE_HOME_DIRECTORY}/bin/unreal/SteamAudioFMODStudio)")
    install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/SteamAudioFMODStudio.uplugin DESTINATION unreal/SteamAudioFMODStudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/Source DESTINATION unreal/SteamAudioFMODStudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/Content DESTINATION unreal/SteamAudioFMODStudio)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/SteamAudioUnreal/Plugins/SteamAudioFMODStudio/Resources DESTINATION unreal/SteamAudioFMODStudio)
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Editor-Mac-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Mac-Development.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
    install(CODE "execute_process(COMMAND python copy_files_from_manifest.py -s ${CMAKE_CURRENT_BINARY_DIR}/HostProjectFMOD/Plugins -d ${CMAKE_HOME_DIRECTORY}/bin/unreal -m ${CMAKE_CURRENT_BINARY_DIR}/Manifest-SteamAudioFMODStudio-UE4Game-Mac-Shipping.xml WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}/build)")
endif()