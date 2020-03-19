# md_test
markdown testing
This file links to [linktest.md](./linktest.md).

## RX-target-board-GCC

### Download/install GCC for Renesas RX and e2studio

### Create a new project with GCC
For example, create project: RTK5RX65Ndemo00
- File > New > C/C++ Project > 
- _Templates for New C/C++ Project_
  - Renesas RX > 
  - GCC for Renesas RX C/C++ Executable Project > 
  - Next >
- _New GCC for Renesas RX Executable Project_
  - Project name: RTK5RX65Ndemo00
  - Next >
- _Select toolchain, device & debug settings_
  - Target Device: ... > RX600 > RX65N > RX65N - 100 pin > R5F565NEDxFP_DUAL
  - change dropdown selection of `E1 (RX)` to `E2 Lite (RX)`
  - select Endian setting: Big of Little (default)
  - Next >
- _Select Coding Assistant Settings_
  - Next >
- _Select Additional CPU Options_
  - Next >
- _Select library generator settings_
  - Next >
- _Summary of project "RTK5RX65Ndemo00"_
  - Finish >

### Edit the HardwareDebug-configuration
- Project > Properties >
- _Properties for RTK5RX65Ndemo00_
  - Run/Debug Settings > RTK5RX65Ndemo00 HardwareDebug > Edit... >
- _Edit launch configuration properties_
  - Debugger > Connection Settings >
    - Clock: HOCO
    - Connection with Target Board: change dropdown selection `JTag` to `Fine`
    - Power: change dropdown selection `Yes` to `No`
  - OK > Apply and Close >

### Set the language standard (optional)
- Project > Properties > 
- _Properties for RTK5RX65Ndemo00_
  - C/C++ Build > Settings > Compiler > Source >
  - Language standard: change dropdown selection as required
  - Apply and Close >

### Build the project
- Project > Build Project > 

After editing project files as required, build the project again to incorporate
the latest changes. If errors come up, the build won't succeed and corrections
have to made before a new build will succeed.

Warnings can arise because the standard settings for Workspace and Project are
a bit picky, for example `if(rc=test_error(params ) ) {...}` will complain with
a `possible assignment in condition`, while the programmer surely intended that
the return value from test_error() given to the result code would require some
additional processing in case it indicated an error by being non-zero.
The standard settings for suggesting parentheses also tend to make code less
readable.

If you want to change these settings for the project, use:
- Project > Properties >  C/C++ General > Code Analysis
  - Check `Use project settings` and set them here

Workspace-wide settings for the above can be made in:
- Window > Preferences > _Preferences_ Code Analysis >

### Start a debugging session
- Run > Debug >
  - Confirm a change to `Debug Perspective`

Learn the keyboard function key shortcuts for stepping through the code.

End the debugging session with a right-click in the debug-pane at the project
and choose: `Terminate and Remove`.
- Yes >
