---
title: "A collection of useful coding snippets"
categories:
  - Coding
tags:
  - cheat sheet
date: 2021-12-16
layout: post
---

I believe every programmer has his/her own repository of coding snippets. I used to manage it with OneNote and EverNote. I am going to manage the core part here.

## python

- Iterate through all files in the current folder.

```python
import os

for root, dirs, files in os.walk(os.getcwd()):
    for name in files:
        print(os.path.join(root, name))
```

## MATLAB

- Iterate through all files in the current folder and remove the root folders.

```matlab
dirInput = dir(fpath);
dirInput(ismember({dirInput.name},{'.','..'})) = []; % remove hidden folder
```

- Make directory if not exist (one liner).

```matlab
dirname = "name";
if ~exist(dirname, 'dir'), mkdir(dirname); end
```

- Spherical VOI out of a matrix\
I learned this from Dr. Qianqian Fang's [MCX](http://mcx.space/) demo code.

```matlab
[yy xx zz] = meshgrid(1:60,1:60,1:60);
S = double(sqrt((xx-30).^2+(yy-30).^2+(zz-30).^2)<=15);
```

- To define input arguments with `"name", "value"` pairs

```matlab
function testarg(x, y, varargin)
params.name_1 = 0;
params.name_2 = 0;

input_params = struct(varargin{:});
keys = fieldnames(input_params);
if nargin>2
  for i = 1:length(input_params)
      cur_key = keys{i};
      if isfield(params, cur_key)
          params.(cur_key) = input_params.(cur_key); 
      else
          warning("Unable to recognize '%s'", cur_key)
      end
  end
end
```

```matlab
>>> testarg(x, y, 'name_1', 1)
```

## Imagemagick

- Make gif from image frames.

```powershell
magick convert -delay 10 -loop 0 *.png animated.gif
```

| Flag | Meaning |
| ---  |   ---   |
| `-delay` | Frame duration $x \times 10$ ms |
| `-loop`  | Number of loops, 0 for infinity |

## ffmpeg

- Make mp4 from image frames

```powershell
ffmpeg -framerate 30 -pattern_type glob -i 'Output_%3d.png' -c:v libx264 -pix_fmt yuv420p out.mp4
```

Refer to the [manual](https://ffmpeg.org/ffmpeg-formats.html#image2-1) for information. However, the `-pattern_type glob` command is not supported by Windows.
