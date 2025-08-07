Import('base_env', 'debug_env', 'release_env', 'build_info')
import os
from SCons.Script import Dir, Return

platform = build_info['platform']
architecture = build_info['architecture'] 
compiler = build_info['compiler']
config = build_info['config']
vsproj = build_info['vsproj']

current_env = debug_env if config.lower() == 'debug' else release_env
config_name = config.capitalize()

obj_prefix = f'/Bin-Int/{platform}-{architecture}/{config_name}/'
current_env['OBJPREFIX'] = obj_prefix

imgui_src_dir = Dir('../../Axiom/Vendor/ImGui')
imgui_sources = imgui_src_dir.glob('*.cpp')
current_env.Append(CPPPATH=[imgui_src_dir])
imgui_lib = current_env.StaticLibrary('ImGui', imgui_sources)

imgui_project = None
if vsproj:
    current_env['CPPPATH'] = [Dir(path) for path in current_env['CPPPATH']]

    imgui_project = current_env.MSVSProject(
        target='../../Axiom/Vendor/ImGui/ImGui' + current_env['MSVSPROJECTSUFFIX'],
        srcs=[],
        buildtarget=imgui_lib,
        variant=[f'{config_name}|x64'],
        auto_build_solution=0
    )

Return('imgui_lib', 'imgui_project')