add_library(CemuWxGui 
  canvas/IRenderCanvas.h
  canvas/OpenGLCanvas.cpp
  canvas/OpenGLCanvas.h
  canvas/VulkanCanvas.cpp
  canvas/VulkanCanvas.h
  CemuApp.cpp
  CemuApp.h
  CemuUpdateWindow.cpp
  CemuUpdateWindow.h
  ChecksumTool.cpp
  ChecksumTool.h
  components/TextList.cpp
  components/TextList.h
  components/wxDownloadManagerList.cpp
  components/wxDownloadManagerList.h
  components/wxGameList.cpp
  components/wxGameList.h
  components/wxInputDraw.cpp
  components/wxInputDraw.h
  components/wxLogCtrl.cpp
  components/wxLogCtrl.h
  components/wxTitleManagerList.cpp
  components/wxTitleManagerList.h
  debugger/BreakpointWindow.cpp
  debugger/BreakpointWindow.h
  debugger/DebuggerWindow2.cpp
  debugger/DebuggerWindow2.h
  debugger/DisasmCtrl.cpp
  debugger/DisasmCtrl.h
  debugger/DumpCtrl.cpp
  debugger/DumpCtrl.h
  debugger/DumpWindow.cpp
  debugger/DumpWindow.h
  debugger/ModuleWindow.cpp
  debugger/ModuleWindow.h
  debugger/RegisterWindow.cpp
  debugger/RegisterWindow.h
  debugger/SymbolCtrl.cpp
  debugger/SymbolCtrl.h
  debugger/SymbolWindow.cpp
  debugger/SymbolWindow.h
  dialogs/CreateAccount/wxCreateAccountDialog.cpp
  dialogs/CreateAccount/wxCreateAccountDialog.h
  dialogs/SaveImport/SaveImportWindow.cpp
  dialogs/SaveImport/SaveImportWindow.h
  dialogs/SaveImport/SaveTransfer.cpp
  dialogs/SaveImport/SaveTransfer.h
  AudioDebuggerWindow.cpp
  AudioDebuggerWindow.h
  DownloadGraphicPacksWindow.cpp
  DownloadGraphicPacksWindow.h
  GameProfileWindow.cpp
  GameProfileWindow.h
  GameUpdateWindow.cpp
  GameUpdateWindow.h
  GeneralSettings2.cpp
  GeneralSettings2.h
  GettingStartedDialog.cpp
  GettingStartedDialog.h
  GraphicPacksWindow2.cpp
  GraphicPacksWindow2.h
  wxWindowSystem.cpp
  helpers/wxControlObject.h
  helpers/wxCustomData.h
  helpers/wxCustomEvents.cpp
  helpers/wxCustomEvents.h
  helpers/wxHelpers.cpp
  helpers/wxHelpers.h
  helpers/wxLogEvent.h
  helpers/wxWayland.cpp
  helpers/wxWayland.h
  input/HotkeySettings.cpp
  input/HotkeySettings.h
  input/InputAPIAddWindow.cpp
  input/InputAPIAddWindow.h
  input/InputSettings2.cpp
  input/InputSettings2.h
  input/PairingDialog.cpp
  input/PairingDialog.h
  input/panels/ClassicControllerInputPanel.cpp
  input/panels/ClassicControllerInputPanel.h
  input/panels/InputPanel.cpp
  input/panels/InputPanel.h
  input/panels/ProControllerInputPanel.cpp
  input/panels/ProControllerInputPanel.h
  input/panels/VPADInputPanel.cpp
  input/panels/VPADInputPanel.h
  input/panels/WiimoteInputPanel.cpp
  input/panels/WiimoteInputPanel.h
  input/settings/DefaultControllerSettings.cpp
  input/settings/DefaultControllerSettings.h
  input/settings/WiimoteControllerSettings.cpp
  input/settings/WiimoteControllerSettings.h
  LoggingWindow.cpp
  LoggingWindow.h
  MainWindow.cpp
  MainWindow.h
  MemorySearcherTool.cpp
  MemorySearcherTool.h
  PadViewFrame.cpp
  PadViewFrame.h
  TitleManager.cpp
  TitleManager.h
  wxCemuConfig.cpp
  wxCemuConfig.h
  EmulatedUSBDevices/EmulatedUSBDeviceFrame.cpp
  EmulatedUSBDevices/EmulatedUSBDeviceFrame.h
  windows/PPCThreadsViewer
  windows/PPCThreadsViewer/DebugPPCThreadsWindow.cpp
  windows/PPCThreadsViewer/DebugPPCThreadsWindow.h
  windows/TextureRelationViewer
  windows/TextureRelationViewer/TextureRelationWindow.cpp
  windows/TextureRelationViewer/TextureRelationWindow.h
  wxcomponents/checktree.cpp
  wxcomponents/checktree.h
  wxgui.h
  wxHelper.h
)

set_property(TARGET CemuWxGui PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")


target_include_directories(CemuWxGui PUBLIC "../")
# PUBLIC because rapidjson/document.h is included in ChecksumTool.h
target_include_directories(CemuWxGui PUBLIC ${RAPIDJSON_INCLUDE_DIRS})

target_link_libraries(CemuWxGui PRIVATE
	CemuAudio
	CemuCafe
	CemuCommon
	CemuComponents
	CemuConfig
	CemuInput
	CemuResource
	CemuUtil
	Boost::headers
	CURL::libcurl
	libzip::zip
	OpenSSL::Crypto
	pugixml::pugixml
	ZArchive::zarchive
)

if(ENABLE_WXWIDGETS AND UNIX AND NOT APPLE)
	# PUBLIC because gdk/gdkkeysyms.h is included in guiWrapper.h
	target_link_libraries(CemuWxGui PUBLIC GTK3::gtk)
  if (ENABLE_WAYLAND)
    target_link_libraries(CemuWxGui PRIVATE Wayland::Client CemuWaylandProtocols)
  endif()
endif()

if(ENABLE_CUBEB)
	target_link_libraries(CemuWxGui PRIVATE cubeb::cubeb)
endif()

if(UNIX AND NOT APPLE)
	if(ENABLE_FERAL_GAMEMODE)
		target_link_libraries(CemuWxGui PRIVATE gamemode)
	endif()
endif()
if (ENABLE_WXWIDGETS)
	# PUBLIC because wx/app.h is included in CemuApp.h
	target_link_libraries(CemuWxGui PUBLIC wx::base wx::core wx::gl wx::propgrid wx::xrc)
endif()

if(WIN32)
	target_link_libraries(CemuWxGui PRIVATE bthprops)
endif()

if(ALLOW_PORTABLE)
	target_compile_definitions(CemuWxGui PRIVATE CEMU_ALLOW_PORTABLE)
endif ()