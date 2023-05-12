# PortMaster - Löve Binaries

This repository provides compile instructions for each love2d version and some runtime notes

# Löve 11.4

- Built using Ubuntu 20.04 LTS aarch64 chroot
```
wget https://github.com/love2d/love/releases/download/11.4/love-11.4-linux-src.tar.gz
tar xf love-11.4-linux-src.tar.gz
cd love-11.4/
./configure
make -j12
strip src/.libs/liblove-11.4.so
copy src/.libs/liblove-11.4.so device/libs
copy src/.libs/love device/
```

# Löve 0.10.2
- Built using Ubuntu 20.04 LTS aarch64 chroot
```
wget https://github.com/love2d/love/releases/download/0.10.2/love-0.10.2-linux-src.tar.gz
tar xf love-0.10.2-linux-src.tar.gz
cd love-0.10.2
./configure
make -j12
strip src/.libs/liblove.so.0
copy src/.libs/lliblove.so.0 device/libs
copy src/.libs/love device/
```

# Löve 0.9.2
- Built using Ubuntu 20.04 LTS aarch64 chroot
- Uses experimental love2d gles build
```
git clone https://github.com/Cebion/love_0.9.2_GLES.git
cd love_0.9.2_GLES
platform/unix/automagic
./configure
make CFLAGS="-Wno-format-overflow"
```
Requires the ENV variable LOVE_GRAPHICS_USE_OPENGLES=1 on launch

# Löve 0.8
- Built using Debian Stretch aarch64 chroot
- Needs boot.lua fixes to set fullscreen in the love binary before compiling
```
wget https://github.com/love2d/love/archive/refs/tags/0.8.0.tar.gz
tar xf 0.8.0.tar.gz
cd love_0.8.0
platform/unix/automagic
./configure
```
Edit src/scripts/boot.lua

Change:
```
		screen = {
			width = 800,
			height = 600,
			fullscreen = false,
			vsync = true,
			fsaa = 0,
		},
 ```
to 

```
		screen = {
			width = 640,
			height = 480,
			fullscreen = true,
			vsync = true,
			fsaa = 0,
		},
 ```
Afterwards 
```
cd src/scripts/
lua auto.lua boot
cd../../
make
```

# Löve 0.7.2
- Built using Debian Stretch aarch64 chroot
- Needs boot.lua fixes to set fullscreen in the love binary before compiling
```
wget https://github.com/love2d/love/archive/refs/tags/0.8.0.tar.gz
tar xf 0.8.0.tar.gz
cd love_0.8.0
platform/unix/automagic
./configure
```
Edit src/scripts/boot.lua

Change:
```
		screen = {
			width = 800,
			height = 600,
			fullscreen = false,
			vsync = true,
			fsaa = 0,
		},
 ```
to 

```
		screen = {
			width = 640,
			height = 480,
			fullscreen = true,
			vsync = true,
			fsaa = 0,
		},
 ```
Afterwards 
```
cd src/scripts/
lua auto.lua boot
cd../../
make
```
# Notes about missing cursor in KMS/DRM
You can add a software cursor for that

KMS/DRM or FBDEV by default does not have a mouse cursor. 

Because of that we have to create a software cursor for each game. 
It's a simple process

Download a cursor 
example:
https://raw.githubusercontent.com/medeirosT/adwaita-2bit-cusors/main/left_ptr.png
  and place it in the game directory root.

Edit the main.lua 

1.  Edit the function love.load() and add
    -- Load your cursor image
    
``` cursorImage = love.graphics.newImage("left_ptr.png")```

 2. Hide the system cursor in the love.load() function: 
 3. 
```love.mouse.setVisible(false) ```

3. Draw the custom cursor image in a new drawCursor function and call it at the end of the love.draw() function:

```
function drawCursor()
    local mouseX, mouseY = love.mouse.getPosition()
    love.graphics.draw(cursorImage, mouseX, mouseY)
end

function love.draw()
    -- Rest of the code

    -- Draw the custom cursor
    drawCursor()
end
```

4. Add cursor scaling to love.update()
 ```
 local xOffset = (love.graphics.getWidth() - simpleScale.getScale() * gw) / 2
  mx = (love.mouse.getX() - xOffset) / simpleScale.getScale()
  my = love.mouse.getY() / simpleScale.getScale()
```
 



