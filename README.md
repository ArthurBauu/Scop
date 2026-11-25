*This project has been created as part of the 42 curriculum by Arbaudou.*

## Description

Scop is a C++17 and OpenGL 3.3 OBJ viewer. This first version parses vertices,
texture coordinates and polygon faces, centers the model, and renders it in
perspective. Faces are displayed in shades of gray and can transition to a
texture loaded from a PPM image, or to a procedurally generated smiley by
default. SDL2 is used for the window and input.

## Instructions

On Debian/Ubuntu, check for the build dependencies and install them if missing:

```sh
command -v c++ >/dev/null && command -v make >/dev/null && command -v pkg-config >/dev/null && pkg-config --exists sdl2 gl || (sudo apt-get update && sudo apt-get install -y build-essential pkg-config libsdl2-dev libgl1-mesa-dev)
```

A working graphical session and an OpenGL 3.3 compatible driver are required.
Build and run from the project directory:

```sh
make
./scop                  # 42 logo
./scop assets/cube.obj  # development fixture
./scop resources/teapot.obj
./scop resources/teapot2.obj
./scop path/to/model.obj
./scop resources/42.obj assets/smiley.ppm
./scop path/to/model.obj path/to/texture.ppm
make test               # OBJ parser and resource tests
```

Controls use physical key positions:

- Arrow keys: rotate around X and Y.
- Q / E: rotate around Z.
- W / S, A / D, R / F: translate along Y, X and Z.
- Space: toggle automatic rotation.
- T: toggle the texture with a smooth transition.
- Home: reset position and orientation.
- Escape: quit.

Pass a texture as the second argument and press T to display it. The image
must be a P3 (text) or P6 (binary) PPM with RGB channels, a maximum value of
1 to 255, and dimensions of at most 8192 pixels per side, subject to the
graphics driver's limit. Transparency is not supported. Without a texture
argument, the program uses its generated smiley.

For example, with ImageMagick installed separately, convert an image to PPM:

```sh
magick image.png -background white -alpha remove -alpha off -depth 8 texture.ppm
```

On ImageMagick 6, use `convert` instead of `magick`. ImageMagick is optional
and is not used by the program.

The official 42 logo and two teapot models are in `resources/`. The 42 logo is
copyrighted and may only be used here for educational purposes. Faces are
currently triangulated as a fan, so concave and non-coplanar polygons are not
guaranteed to render correctly. Generated texture coordinates are a basic
per-face projection, not the texture-mapping bonus. Unsupported OBJ records,
including materials and normals, are ignored. Missing files and invalid vertex,
texture coordinate or face data are reported on standard error.

## Resources

- [OpenGL documentation](https://registry.khronos.org/OpenGL/)
- [SDL2 documentation](https://wiki.libsdl.org/SDL2/FrontPage)
- [Wavefront OBJ format](https://www.loc.gov/preservation/digital/formats/fdd/fdd000507.shtml)

GitHub Copilot was used to analyze the subject and assist with the initial
implementation, tests, and documentation.