# FreeCAD Knurl Macro

**Note**: This macro modifies your geometry destructively. Always work on a copy of your original design or save before running the macro!

---

A comprehensive FreeCAD macro for creating professional knurl patterns on cylindrical surfaces. This macro provides an intuitive GUI interface for generating various types of knurls including diamond (crosshatch), straight, and diagonal patterns.

![License](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)

## License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**. 

This means you are free to:
- Use this macro for any purpose
- Modify and improve the code
- Distribute copies
- Distribute your modified versions

**However**, if you run a modified version of this macro on a server or provide it as a service, you **must** make your modified source code available under the same AGPL-3.0 license.

See the [LICENSE](LICENSE) file for full details or visit https://www.gnu.org/licenses/agpl-3.0.html

## Version history

### December 27 2025

**Version 0.1.0**
Initial work to provide a knurling macro for FreeCAD.


## Features

### Knurl Pattern Types
- **Diamond Knurl (Cross Pattern)** - Most common type with excellent all-direction grip
- **Straight Knurl** - Parallel lines running along the cylinder axis
- **Diagonal Knurl** - Single direction angled pattern
- **Left-Hand Diagonal** - Diagonal pattern angled to the left
- **Right-Hand Diagonal** - Diagonal pattern angled to the right

### Tooth Profile Options
- **Sharp (V-Profile)** - Aggressive grip with pointed peaks
- **Rounded** - Comfortable touch, ideal for consumer products
- **Flat-Top** - Durable and less aggressive, common on thumbscrews

### Pitch (Tooth Spacing) Presets
- **Fine (1.0mm)** - For precision parts and small diameters
- **Medium (2.0mm)** - General purpose applications
- **Coarse (3.0mm)** - Heavy-duty grip requirements
- **Extra Coarse (4.0mm)** - Industrial applications
- **Custom** - User-defined pitch value

### Adjustable Parameters
- **Knurl Depth** (0.1-5.0mm) - How deep the grooves cut into the surface
- **Knurl Angle** (15-75°) - Angle of diagonal patterns (typically 30-45° for diamond)
- **Band Width/Height** (1-100mm) - Height of the knurl band when selecting an edge

## Installation

1. Download `knurl.macro` from this repository
2. In FreeCAD, go to **Macro → Macros...**
3. Click **User macros location** button to open your macros folder
4. Copy `knurl.macro` into this folder
5. Restart FreeCAD or click **Refresh** in the Macro dialog

## Usage

### Basic Workflow

1. **Select Geometry**
   - Select a **circular edge** (e.g., the rim of a cylinder) for a localized knurl band
   - OR select a **cylindrical face** to knurl the entire surface
   - OR select a **cylindrical solid** and it will automatically find the cylindrical face

2. **Run the Macro**
   - Go to **Macro → Macros...** and select `knurl.macro`
   - Click **Execute**

3. **Configure Parameters** in the dialog that appears:
   - Choose your knurl **Pattern Type**
   - Select the **Tooth Profile**
   - Set the **Pitch** (tooth spacing)
   - Adjust the **Depth** (how deep to cut)
   - Set the **Angle** (for diamond/diagonal patterns)
   - Set the **Band Width** (height of knurl when using edge selection)

4. **Click "Apply Knurl"**
   - The macro will process the cuts in batches
   - Progress is shown in the Python console
   - A new "Knurled" object will be created
   - The original object is hidden (not deleted) for comparison

### Selection Methods

#### Edge Selection (Recommended for controlled areas)
When you select a circular edge:
- The **Band Width** parameter controls the height of the knurl band
- Default is 10mm, adjust as needed
- Perfect for adding grip to specific areas like knobs or handles

#### Face Selection (For complete coverage)
When you select a cylindrical face:
- The entire face height is knurled automatically
- **Band Width** parameter is ignored
- Good for full-length cylinder knurling

### Tips for Best Results

1. **Start with Medium settings**: Use Medium pitch (2.0mm) and 0.6mm depth
2. **Increase depth for visibility**: If knurl is too shallow, increase depth to 1.0-1.5mm
3. **Diamond pattern works best**: The crosshatch provides the best grip
4. **Adjust angle carefully**: 35° is ideal for diamond patterns
5. **Test on a copy first**: The macro modifies the geometry significantly

## How It Works

### Technical Overview

The macro creates knurls by:

1. **Geometry Detection**
   - Analyzes the selected object to find circular edges or cylindrical faces
   - Extracts cylinder properties: radius, center point, axis direction

2. **Cut Tool Generation**
   - Creates rectangular cutting boxes based on the selected pattern
   - For **Diamond knurl**: generates pairs of boxes rotated at +angle and -angle
   - For **Straight knurl**: generates boxes parallel to the cylinder axis
   - For **Diagonal knurl**: generates boxes tilted in one direction

3. **Transformation**
   - Positions each cutting box at the cylinder surface
   - Rotates boxes to create the helical groove pattern
   - Distributes cuts evenly around the circumference based on pitch

4. **Boolean Operations**
   - Applies cutting boxes to the shape using boolean cut operations
   - Processes in batches of 20 to maintain performance
   - Cleans up the result with `removeSplitter()`

5. **Result**
   - Creates a new "Knurled" object with the pattern applied
   - Hides the original object for comparison

### Pattern Generation

- **Diamond/Crosshatch**: Two sets of helical cuts at opposite angles create the characteristic diamond pattern
- **Straight**: Parallel grooves along the cylinder axis
- **Diagonal**: Single set of helical cuts at the specified angle

## Troubleshooting

### No knurl appears
- Increase the **Depth** parameter (try 1.0mm or more)
- Check that your selection is a valid cylindrical surface
- Verify in the console that cuts are being applied (volume should change)

### Knurl is too fine/coarse
- Adjust the **Pitch** parameter
- Smaller pitch = more teeth, finer pattern
- Larger pitch = fewer teeth, coarser pattern

### Edges look rough or broken
- The cutting boxes should now match the band height exactly
- Ensure you're using the latest version of the macro
- Try adjusting the band width to better match your geometry

### Macro runs slowly
- Reduce the pitch (fewer cuts to process)
- The macro processes cuts in batches for performance
- Large cylinders with fine pitch will take longer

### Only part of cylinder is knurled
- When selecting a **face**, it knurls the entire face bounds
- When selecting an **edge**, it uses the Band Width parameter
- Make sure you select the correct geometry type for your needs

## Debug Mode

The macro includes a debug visualization mode for troubleshooting:

1. Edit the macro file
2. Find line ~380: `ENABLE_DEBUG = False`
3. Change to: `ENABLE_DEBUG = True`
4. Run the macro
5. The cutting tools will be visualized in red and blue colors
6. For diamond patterns: red = right-hand cuts, blue = left-hand cuts

This helps verify the cutting pattern before applying it to your geometry.

## Requirements

- FreeCAD 0.19 or later
- Python 3.x (included with FreeCAD)
- Part Design workbench

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

When contributing, please:
- Follow the existing code style
- Test your changes thoroughly
- Update documentation as needed
- Ensure your contributions are compatible with AGPL-3.0

## Known Limitations

- Works best on simple cylindrical geometries
- Complex curved surfaces may not knurl correctly
- Very fine pitches (<0.5mm) may cause performance issues
- Boolean operations can be slow on complex geometries

## Future Enhancements

Potential improvements for future versions:
- Support for conical surfaces
- Helical knurl patterns
- Micro knurl patterns
- Custom tooth profiles
- Batch processing multiple objects
- Export/import knurl presets

## Author

**Gary Foster (dafogary) - DAFO Creative Ltd/LLC** - Initial work

<a href="https://dafocreative.com">
  <img src="https://dafocreative.com/wp-content/uploads/2021/08/DAFO-logo-copy.png" alt="DAFO Creative" width="300"/>
</a>

## Acknowledgments

- FreeCAD community for excellent documentation
- All contributors and testers

## Support

If you encounter issues or have questions:

1. Check the Troubleshooting section above
2. Enable Debug Mode to visualize the cutting pattern
3. Check the FreeCAD Python console for error messages
4. Open an issue on GitHub with details about your setup and the problem

---

**Note**: This macro modifies your geometry destructively. Always work on a copy of your original design or save before running the macro!
