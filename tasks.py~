###############################################
#                Imports                      #
###############################################
import os
import subprocess

from invoke import task

###############################################
#                Public API                   #
###############################################
ROOT_PATH = os.path.dirname(os.path.abspath(__file__))
KICAD_SRC_PATH = os.path.join(ROOT_PATH, "kicad_src")
OPENSCAD_SRC_PATH = os.path.join(ROOT_PATH, "openscad_src")
BUILD_PATH = os.path.join(ROOT_PATH, "build")
OPENSCAD_LIBS_PATH = os.path.join(BUILD_PATH, "openscad_libs")


@task
def clean(c, bytecode=False, extra=""):
    """
    Clean up build and temporary files.

    This task removes the specified patterns of files and directories,
    including build artifacts, temporary files, and optionally Python
    bytecode files.

    Args:
        bytecode (bool, optional): If True, also removes Python bytecode files (.pyc). Defaults to False.
        extra (str, optional): Additional pattern to remove. Defaults to "".

    Usage:
        inv clean
        inv clean --bytecode
        inv clean --extra='**/*.log'
    """
    patterns = ["build/*", "**/*~", "**/#*", "*~", "#*", "**/*.stl"]

    if bytecode:
        patterns.append("**/*.pyc")
    if extra:
        patterns.append(extra)

    for pattern in patterns:
        _pr_info("Removing pattern {}".format(pattern))
        c.run("rm -vrf {}".format(pattern))


@task
def open_kicad(c):
    """
    Open KiCad project.

    Usage:
        inv open-kicad
    """
    CC = "kicad"
    project_name = "rgb_led_toy.kicad_pro"
    project_file = os.path.join(KICAD_SRC_PATH, project_name)

    if not os.path.isfile(project_file):
        _pr_error(f"Project file {project_file} does not exist!")
        return

    if not _command_exists(CC):
        _pr_error(f"{CC} needs to be installed!")
        return

    _pr_info(f"Opening {project_file} in KiCad")

    command = f"{CC} {project_file}"
    print(command)
    c.run(command)


@task
def open_openscad(
    c, file_path=os.path.join(OPENSCAD_SRC_PATH, "enclosure_part_1.scad")
):
    """
    Open openscad project.

    Usage:
        inv open-openscad
    """
    CC = "openscad"
    _write_libs_path_to_envs()

    if not os.path.isfile(file_path):
        _pr_error(f"File {file_path} does not exist!")
        return

    if _get_file_extension(file_path) != ".scad":
        _pr_error(f"File {file_path} is not an OpenSCAD file!")
        return

    if not _command_exists(CC):
        _pr_error(f"{CC} needs to be installed!")
        return

    _pr_info(f"Opening {file_path} in OpenSCAD")

    command = f"{CC} {file_path}"
    c.run(command)


@task
def build(c):
    """
    Build the entire project: KiCad and OpenSCAD.

    This function builds both the KiCad project (Gerber and drill files)
    and the OpenSCAD project (STL files).

    Usage:
        inv build
    """
    try:
        download_deps(c)
    except Exception:
        _pr_error("Downloading dependencies failed!")
        return

    try:
        build_kicad(c)
    except Exception:
        _pr_error("Building KiCad project failed!")
        return

    try:
        build_openscad(c)
    except Exception:
        _pr_error("Building OpenSCAD project failed!")
        return


@task
def build_kicad(c):
    """
    Build KiCad project: generate Gerber and drill files.

    This task assumes `kicad-cli` is installed and available in the PATH.

    Usage:
        inv build-kicad
    """
    project_name = (
        "rgb_led_toy.kicad_pcb"  # Change if your PCB file has a different name
    )
    pcb_file = os.path.join(KICAD_SRC_PATH, project_name)
    output_path = os.path.join(BUILD_PATH, "kicad_output")

    if not os.path.isfile(pcb_file):
        _pr_error(f"PCB file {pcb_file} does not exist!")
        raise ValueError()

    if not _command_exists("kicad-cli"):
        _pr_error("kicad-cli is not installed or not in PATH!")
        raise ValueError()

    # Create the output directory if it doesn't exist
    os.makedirs(output_path, exist_ok=True)

    # Generate Gerber files
    _pr_info("Generating Gerber files...")
    gerber_command = f"kicad-cli pcb export gerbers --output {output_path} {pcb_file}"
    try:
        c.run(gerber_command)
        _pr_info("Gerber files generated successfully.")
    except Exception as e:
        _pr_error(f"Failed to generate Gerber files: {e}")
        raise e

    # Generate drill files
    _pr_info("Generating drill files...")
    drill_command = f"kicad-cli pcb export drill --output {output_path} {pcb_file}"
    try:
        c.run(drill_command)
        _pr_info("Drill files generated successfully.")
    except Exception as e:
        _pr_error(f"Failed to generate drill files: {e}")
        raise e

    _pr_info(f"KiCad project built successfully. Output saved to {output_path}.")


@task
def build_openscad(c):
    """
    Build OpenSCAD project: generate STL files from .scad files.

    Usage:
        inv build-openscad
    """
    output_path = os.path.join(BUILD_PATH, "openscad_output")
    _write_libs_path_to_envs()

    if not _command_exists("openscad"):
        _pr_error("OpenSCAD is not installed or not in PATH!")
        raise ValueError("OpenSCAD command not found.")

    # List all .scad files in the OpenSCAD source directory
    scad_files = _list_scad_files_recursively(OPENSCAD_SRC_PATH)

    # Ensure the build directory exists
    os.makedirs(BUILD_PATH, exist_ok=True)
    # Create the output directory if it doesn't exist
    os.makedirs(output_path, exist_ok=True)

    # Generate STL files
    for file_path in scad_files:
        output_file = os.path.join(
            output_path, os.path.basename(file_path).replace(".scad", ".stl")
        )
        _pr_info(f"Generating STL for {file_path} -> {output_file}")

        # Run the OpenSCAD command
        command = f"openscad -o {output_file} {file_path}"
        try:
            c.run(command)
            _pr_info(f"Generated STL: {output_file}")
        except Exception as e:
            _pr_error(f"Failed to generate STL for {file_path}: {e}")
            raise e

    _pr_info("OpenSCAD project built successfully.")


@task
def download_deps(c):
    """
    Download all dependencies required to build the project.

    Usage:
        inv download-deps
    """
    _download_bosl2(c)


def _download_bosl2(c):
    _pr_info("Checking BOSL2 library...")
    if not _command_exists("git"):
        _pr_error("Git is not installed or not available in the PATH.")
        return

    branch_or_commit = "c442c5159ae605dfe5d4f0262d521aeae02ea6c3"
    bosl2_path = os.path.join(OPENSCAD_LIBS_PATH, "BOSL2")

    # Ensure the OpenSCAD libraries directory exists
    os.makedirs(OPENSCAD_LIBS_PATH, exist_ok=True)

    if os.path.exists(bosl2_path) and os.path.isdir(bosl2_path):
        _pr_info(f"BOSL2 repository found at {bosl2_path}. Doing nothing...")
    else:
        # Clone the repository if it doesn't exist
        repo_url = "https://github.com/BelfrySCAD/BOSL2.git"
        clone_command = f"git clone {repo_url} {bosl2_path}"
        _pr_info(f"Cloning BOSL2 repository to {bosl2_path}...")
        try:
            c.run(clone_command)
            with c.cd(bosl2_path):
                c.run(f"git checkout {branch_or_commit}")
            _pr_info(f"BOSL2 repository cloned and checked out to {branch_or_commit}.")
        except Exception as e:
            _pr_error(f"Failed to download or checkout BOSL2 library: {e}")


def _write_libs_path_to_envs():
    os.environ["OPENSCADPATH"] = OPENSCAD_LIBS_PATH


###############################################
#                Private API                   #
###############################################
def _get_file_extension(file_path):
    _, file_extension = os.path.splitext(file_path)
    return file_extension


def _command_exists(command):
    """
    Check if a command exists using 'which'.

    Args:
        command (str): The command to check.

    Returns:
        bool: True if the command exists, False otherwise.
    """
    try:
        result = subprocess.run(
            ["which", command],
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            check=True,
        )
        return result.returncode == 0
    except FileNotFoundError:
        return False
    except subprocess.CalledProcessError:
        # If 'which' runs but doesn't find the command, return False
        return False
    except Exception as e:
        # Log unexpected errors
        _pr_error(f"Error checking command {command} with 'which': {e}")
        return False


def _cut_path_to_directory(full_path, target_directory):
    """
    Cuts the path up to the specified target directory.

    :param full_path: The full path to be cut.
    :param target_directory: The directory up to which the path should be cut.
    :return: The cut path if the target directory is found, otherwise raises ValueError.
    """
    parts = full_path.split(os.sep)

    target_index = parts.index(target_directory)
    return os.sep.join(parts[: target_index + 1])


def _list_scad_files_recursively(root_dir):
    _SCAD_EXTENSION = ".scad"
    files = []

    for dirpath, dirnames, filenames in os.walk(root_dir):
        for filename in filenames:
            if "_" == filename[:1]:
                continue
            if _SCAD_EXTENSION == _get_file_extension(filename):
                files.append(os.path.join(dirpath, filename))

    return sorted(files)


def _pr_info(message: str):
    """
    Print an informational message in blue color.

    Args:
        message (str): The message to print.

    Usage:
        pr_info("This is an info message.")
    """
    print(f"\033[94m[INFO] {message}\033[0m")


def _pr_warn(message: str):
    """
    Print a warning message in yellow color.

    Args:
        message (str): The message to print.

    Usage:
        pr_warn("This is a warning message.")
    """
    print(f"\033[93m[WARN] {message}\033[0m")


def _pr_debug(message: str):
    """
    Print a debug message in cyan color.

    Args:
        message (str): The message to print.

    Usage:
        pr_debug("This is a debug message.")
    """
    print(f"\033[96m[DEBUG] {message}\033[0m")


def _pr_error(message: str):
    """
    Print an error message in red color.

    Args:
        message (str): The message to print.

    Usage:
        pr_error("This is an error message.")
    """
    print(f"\033[91m[ERROR] {message}\033[0m")


if __name__ == "__main__":
    _write_libs_path_to_envs()
