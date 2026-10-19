# ELEX 4618 Programs

[Project portfolio](https://fyemane.github.io/projects.html) · [About me, experience & qualifications](https://fyemane.github.io/)


My object-oriented C++ projects for ELEX 4618 at BCIT. I built these applications in Windows with Visual Studio, using OpenCV for graphics and image processing and a TI TM4C123G LaunchPad with the Educational BoosterPack MKII for physical input and output.

The projects progress from embedded I/O and a drawing application to Pong, my asteroid game **C Plus Destroy**, and camera/networking experiments. I later ported applications to Linux on Raspberry Pi 4B. I keep that work, including my Pi-based colour ball sorter, in [ELEX4618-RaspberryPi](https://github.com/fyemane/ELEX4618-RaspberryPi).

## Projects

### Etch-A-Sketch

I used the BoosterPack joystick to move a drawing cursor across an OpenCV canvas. The application keeps the drawing between frames, cycles pen colours with a physical pushbutton, and supports clearing the canvas through a reset button. The on-screen interface also provides Reset and Exit controls.

Main files: `CSketch.cpp` and `CSketch.h`.

![Etch-A-Sketch](https://github.com/fyemane/ELEX4618-Programs/assets/93152842/a3b9f654-9cf8-4e03-83e1-b31546f30bce)

### Pong

My Pong game puts a joystick-controlled paddle against a computer opponent. It includes ball movement, paddle and boundary collisions, score tracking, and a game-ending score of five. I added sliders for ball size and speed, an FPS display, and Reset and Exit buttons. A physical button also triggers the reset logic.

Main files: `CPong.cpp` and `CPong.h`.

![Pong](https://github.com/fyemane/ELEX4618-Programs/assets/93152842/89e302b7-c4de-4bc2-a6a1-e07ca6833a3a)

### C Plus Destroy

My take on Asteroids expands the game into a set of interacting C++ objects: the player ship, asteroids, enemies, player and enemy missiles, and collectible items. It includes progressive levels, timed enemy spawning, collisions, score tracking, lives, health, a game-over state, and background music through the Windows multimedia API.

The game reads the joystick for ship orientation and the BoosterPack buttons for firing and thrust. Button 1 starts the game from the title screen. The interface also includes Reset and Exit controls.

Main files: `CAsteroidGame`, `CGameObject`, `CShip`, `CAsteroid`, `CEnemy`, `CMissile`, `CEnemyMissile`, and `CItem`.

![C Plus Destroy](https://github.com/fyemane/ELEX4618-Programs/assets/93152842/eda81f90-2dd2-4c88-9cb0-fd829f1d1889)

[Watch my C Plus Destroy playthrough](https://www.youtube.com/watch?v=uuwGinJpxHk).

### Embedded control and image/networking demos

The console menu also exposes analogue input, digital I/O, pushbutton and servo tests, serial communication, still-image manipulation, camera processing, and client/server demonstrations. The camera demo includes Canny filtering and ArUco marker detection.

## How the code is organized

| Component | Purpose |
| --- | --- |
| `4618_Template.cpp` | Main console menu and lab/demo entry points |
| `CBase4618` | Shared application interface with update, draw, and run methods |
| `CControl` | Embedded I/O requests and microcontroller detection |
| `Serial` | Windows serial-port access |
| `CSketch`, `CPong`, `CAsteroidGame` | Application logic and rendering |
| `CGameObject` and derived classes | Game entities, movement, and interactions |
| `cvui.h` | Buttons, sliders, and other controls drawn in OpenCV windows |
| `Client`, `server`, `4618_Client.cpp` | Socket communication demonstrations and client entry point |
| `stdafx.h` | Shared includes and OpenCV library-link settings |

The Windows hardware path is **application → CControl → Serial → LaunchPad/BoosterPack**. The text protocol uses `G` requests to read inputs, `S` requests to set outputs, and acknowledgement responses. Application code calls the control class rather than handling the serial port directly.

## Hardware and software

- Windows PC with Visual Studio's Desktop development with C++ tools.
- The checked-in project targets the **v143 toolset**, Windows SDK 10.0, and x64 configurations.
- OpenCV with the modules used by the project, including ArUco. The current linker settings reference **OpenCV 4.5.0** libraries under `opencv/x64/vc16/staticlib`.
- TI TM4C123G LaunchPad and Educational BoosterPack MKII, connected over USB, with compatible course firmware for the serial protocol.
- A camera for video demos; speakers or headphones for game audio.

The hardware-driven applications expect the LaunchPad/BoosterPack inputs. I haven't documented a complete keyboard-only mode.

## Building and running on Windows

1. Clone or download this repository and open `4618_Template.sln` in Visual Studio.
2. Install the v143 C++ toolset and Windows SDK, or deliberately retarget the solution to the tools installed on your machine.
3. Supply a compatible OpenCV build. The project looks for headers in `./opencv/include`, and `stdafx.h` lists the library filenames and paths. Update both if using another layout or OpenCV version. Match the architecture and Debug/Release libraries to the selected configuration.
4. Connect the LaunchPad and BoosterPack and confirm that the board is running firmware matching the project's command protocol. `CControl::init_com()` currently calls automatic microcontroller detection; the standalone serial test separately hard-codes `COM3`.
5. Set `4618_Template` as the startup project and build the x64 configuration. Keep the working directory at the repository root so relative image and audio paths resolve.
6. Run the program and choose an application from the console menu.

| Menu option | Application |
| --- | --- |
| 3 | Embedded control tests |
| 4 | Etch-A-Sketch |
| 5 | Pong |
| 6 | C Plus Destroy |
| 10 | Serial COM test |
| 11 | Image manipulation |
| 12 | Video manipulation |
| 13 | Client/server demonstration |
| 0 | Exit the main menu |

Use the application's on-screen controls where provided. Keep the OpenCV window focused for keyboard events; several application loops check for `q`.

## Notes on this version

This repository preserves my course implementation. I haven't verified a fresh build against a current Visual Studio/OpenCV installation. OpenCV binaries and board firmware need to be supplied separately, and the relative library paths may need updating. Several applications also need review of their thread shutdown logic before treating this as a maintained release.

My [Raspberry Pi repository](https://github.com/fyemane/ELEX4618-RaspberryPi) uses a different hardware control layer and a Linux build environment. The ball sorter was developed directly on the Pi and isn't part of this Windows codebase.

## Credits

I built these projects from the BCIT ELEX 4618 course framework. Existing source-file credits are retained. The asteroid-game source includes references for its background image and music. I haven't added a new license or changed the attribution of those assets.
