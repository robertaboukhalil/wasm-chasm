## Crossing the Wasm Chasm Video

### Setup
(skip if you already have Emscripten installed)

```bash
# Download Emscripten container
docker pull robertaboukhalil/emsdk:1.39.1

# Create a container that exposes the container's port 80 as 12345 and mounts the current directory as /src.
# This allows you to load .html, .wasm and .js files in your browser at http://localhost:12345:
docker run \
    -dt \
    -p 12345:80 \
    --name wasm \
    --volume "$(pwd)":/src \
    robertaboukhalil/emsdk:1.39.1

# Go inside the container
docker exec -it wasm bash
```

### Compile to WebAssembly

```bash
# Use Asyncify to address the infinite loop in this app.
# An infinite loop on the main thread crashes the browser tab
cd mandelbrot
git apply ../mandelbrot.diff

# Compile to WebAssembly (enable Asyncify and SDL2 port)
emcc -O2 -o mandelbrot.html mandelbrot.c -s USE_SDL=2 -s ASYNCIFY=1
```

### Further Reading

* [Faster Fractals with Multi-Threaded WebAssembly](https://blog.scottlogic.com/2019/07/15/multithreaded-webassembly.html), by Colin Eberhardt
* [Pause and Resume WebAssembly with Asyncify](https://kripken.github.io/blog/wasm/2019/07/16/asyncify.html), by Alon Zakai
* [A Cartoon intro to WebAssembly](https://hacks.mozilla.org/2017/02/a-cartoon-intro-to-webassembly), by Lin Clark
