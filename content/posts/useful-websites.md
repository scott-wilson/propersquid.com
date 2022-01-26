+++
title = "Useful Websites"
date = 2018-05-26
draft = false
+++

Here's a list of useful programming and CG resources I have found on the internet. This list may get updated in the future.

# Go
- https://awesome-go.com/ A collection of packages for Go.

# Python
- https://tox.readthedocs.io/en/latest/ A really good tool for testing in multiple environments. For example, having code that is guaranteed to work in Python 2.7, 3.x, Maya, Nuke, Houdini, etc.
- https://docs.pytest.org/en/latest/ This makes writing unit tests a lot simpler. I may do a post on writing tests with my "testing trifecta".
- https://hypothesis.works/ Oh boy. This is a great testing tool. It is a property based testing framework that lets you slam your code with a bunch of random data to verify if it works or not. Whenever I write Python tests, I use this library first to get 100% coverage (or as close as I can to), then use regular unit tests to fill in the gaps or cover known breaks.

# Photogrammetry and Texture Generation
- http://culturalheritageimaging.org/Technologies/RTI/ I haven't experimented with this a lot, but it is a way to capture surface information (such as information used for normal maps) using a series of images of the same surface with different lighting conditions.
- https://mediatech.aalto.fi/publications/graphics/TwoShotSVBRDF/ Related to the above link.
- https://lesterbanks.com/2015/12/building-your-own-portable-pbr-texture-scanner/ A rig for capturing images for photorealistic textures.
- http://www.gamasutra.com/blogs/JosephAzzam/20160824/278719/The_Workflows_of_Creating_Game_Ready_Textures_and_Assets_using_Photogrammetry.php A guide on creating photogrammetry assets for games.
- http://www.gamasutra.com/blogs/JosephAzzam/20170110/288899/Everything_You_Need_to_Know_about_Photogrammetry_I_hope.php More useful information on photogrammetry.

# Libraries
- https://github.com/ocornut/imgui An interesting library for creating UIs in a 3d application.
- http://taichi.graphics/ An interesting looking library for doing rendering and simulations.
- https://github.com/OpenImageIO/oiio A great library for doing image reading/writing. It supports a whole lot of formats, and supports 8 bit integers to 32 bit floats.
- https://embree.github.io/ A library for efficient ray tracing on the CPU (not sure about GPU). This library is used in a few path tracer renderers. (I think Cycles, and V-Ray are included.)
- https://wiki.blender.org/index.php/Dev:Shading/Tangent_Space_Normal_Maps A page that contains code for the mikktspace tangent space normal maps.
- http://www.digitalmistakegames.com/2017/07/30/280/ A photogrammetry example for making rubble textures.
- http://mantaflow.com/ A fluid simulator.
- https://www.disneyanimation.com/technology/partio.html A library for reading/writing particles in different formats.
- http://www.tuttleofx.org/ A framework for doing 2d image operations using OpenFX.
- http://www.hxa.name/minilight/ A small renderer.
- https://www.disneyanimation.com/technology/seexpr.html An expression language that can be used in 2d/3d software.
- https://github.com/Psyop/Cryptomatte A library for generating masks for 3d renders.
- https://mltframework.org/ A framework for non-linear editing.
- http://nikitron.cc.ua/sverchok_en.html A library for node based mesh editing in Blender.
- https://www.sofa-framework.org/ A simulations framework.
- https://github.com/PixarAnimationStudios/OpenTimelineIO A library for dealing with editoral timelines.
- https://github.com/materialx/MaterialX A standard for transferring materials between renderers.
- https://www.imageflow.io/ A library for image editing.

# Rendering and Computer Graphics
- https://users.cg.tuwien.ac.at/zsolnai/gfx/separable-subsurface-scattering-with-activision-blizzard/ A method for real time subsurface scattering.
- http://gaps-zju.org/mlchai/ This person has some interesting papers on creating and simulating hair.
- https://pages.mtu.edu/~shene/COURSES/cs3621/NOTES/model/winged-e.html The winged edge data structure for 3d meshes. I could be wrong, but I think it is the same structure Blender uses.
- http://www.vfxplatform.com/ An agreement between studios and software venders on specific versions of libraries, so it will be easier to manage dependencies.
- https://simblob.blogspot.ca/ This blog has some interesting information on real time rendering for games.
- https://gpuopen.com/ A collection of libraries and information on creating real time renders.
- http://www.gdcvault.com/play/1023002/Low-Complexity-High-Fidelity-INSIDE A breakdown on the rendering behind INSIDE.
- http://www.adriancourreges.com/blog/2016/09/09/doom-2016-graphics-study/ A breakdown on the rendering behind 2016's DOOM.
- https://www.gamedev.net/blogs/blog/2031-ray-tracing-devlog/ A developer log on creating a raytracer.
- https://raytracey.blogspot.ca/ A blog about raytracing.
- http://shaderbits.com/blog Some really cool shader information.
- http://pbrt.org/ A book on creating a physically based renderer.
- http://www.iquilezles.org/ More really cool shader stuff.
- http://www.gamasutra.com/blogs/DeannaVanBuren/20151012/254238/Architecture_in_Video_Games_Designing_for_Impact.php A blog post talking about using architects as level developers.
- https://github.com/ampas/aces-dev A set of colour transforms from the AMPAS Academy.
- http://filmicworlds.com/ A blog on colour and colour grading.
- http://www.gamasutra.com/blogs/JohannesKuhlmann/20170327/294031/InHouse_Engine_Development_Technical_Tips.php A blog post on creating in house game engines.
- https://bartwronski.com/ A rendering blog.
- http://www.gamasutra.com/blogs/DougStewart/20170411/295806/Color_in_Video_Games_How_to_Choose_a_Palette.php Picking palettes in video games.
- http://www.gamasutra.com/view/feature/130689/book_excerpt_programming_a_.php This is talking about doing culling for games.
- http://ogldev.org/ OpenGL tutorials.
- http://cvc.ucsb.edu/graphics/Papers/SIGGRAPH2017_KPCN/ Machine learning denoiser.
- http://theorangeduck.com/page/phase-functioned-neural-networks-character-control Machine learning character controller.
- https://simonschreibt.de/game-art-tricks/ Interesting tricks and tips for game rendering.
- http://www.cultofrig.com/ A site about creating rigs for animation.
- http://blog.selfshadow.com/publications/ A collection of papers on rendering.
- https://blendergrid.com/news/cycles-physically-correct-brightness Lighting in Cycles.
- https://belcour.github.io/blog/research/2017/05/01/brdf-thin-film.html A paper for surface rendering.
- https://www.apertus.org/ An open source camcorder.
- https://www.sebastiansylvan.com/post/matrix_naming_convention/ A really useful naming convention for matrixes that I'm starting to follow.
- https://github.com/KhronosGroup/glTF A 3d graphics interchage format.
- http://julius-ihle.de/?page_id=346 A lookdev/lighting TD's blog
- http://www.adriancourreges.com/blog/2017/12/15/mgs-v-graphics-study/ A breakdown of the Metal Gear Solid 5's renderer.
- http://alsber.com/ A blog on a really cool animation rig system.
- https://procworld.blogspot.ca/ A blog on creating procedural worlds.
- http://blog.stevemcauley.com/ Another rendering blog.
- https://simonstechblog.blogspot.ca/ You can tell I like rendering blogs.
- https://interplayoflight.wordpress.com/ More rendering blogs!
- http://fabiensanglard.net/ Another rendering blog.
- https://bitsquid.blogspot.ca/ A game engine blog.
- https://thebookofshaders.com/ A book on shader development.
- https://fredrikaverpil.github.io/ A blog from a Pipeline TD. There's some really useful information in here.

# Tools (Free and/or Open Source)
- http://www.gafferhq.org/ A tool for doing lighting and lookdev for CG art.
- http://mypaint.org/ A painting application with a really cool brush engine.
- https://www.banshee3d.com/ An interesting looking game engine.
- https://www.blender.org/ A 3d animation package
- https://krita.org/ A 2d painting application.
- https://natron.fr/ A compositing application.
- https://ffmpeg.org/ A tool/library for doing audio/video manipulation.
- https://www.mapeditor.org/ A 2d map editor.
- http://www.meshlab.net/ A 3d package for mesh cleanup. Useful for very dense meshes.
- https://colmap.github.io/ A structure from motion tool.
- https://github.com/Unity-Technologies/DeLightingTool A delighting tool for Unity.
- https://luxcorerender.org/ A physically based renderer.
- https://appleseedhq.net/ Another renderer.

# Resources
- https://material.io/tools/icons/?style=baseline Material icons.
- https://www.culturehustle.com/ This site contains the blackest black paint. I find that cool.
- https://community.renderman.pixar.com/article/1787/library-pixar-one-thirty.html Free textures created by Pixar.
- https://hdrihaven.com/ Free HDRI images
- https://www.poliigon.com/ A library of photorealistic textures.
- https://www.eisklotz.com/ Textures and HDRI images.
- http://cc0textures.com/home More textures.
- https://forkawesome.github.io/Fork-Awesome/ A fork of the font awesome library.
