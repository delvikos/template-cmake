# template-cmake

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Build Instructions

### Building with CMake

```bash
# 1. List available presets
cmake --list-presets

# 2. Configure the project (generate build files)
cmake --preset {preset-name}

# 3. Compile the project
cmake --build --preset {preset-name}

# 4. Test the project (debug-presets only)
ctest --preset {preset-name}
```

### Building via IDE

The project natively integrates with modern IDEs supporting the CMake Presets specification.

* **Visual Studio / CLion**: Automatically detects `CMakePresets.json` at the root. Select the target preset from the active configuration dropdown and trigger the **Build** target.
* **VS Code**: Requires the **CMake Tools** extension.
