<!DOCTYPE html>
	<html>
	<head></head>
	<body> <!--style="background-color:lightyellow;-->
		<h1 id="using-glfw-and-simple-opengl-to-create-a-simple-shooter-game">Using GLFW (and simple OpenGL) to create a simple shooter game.</h1>
		<p>OpenGL is the first programming utility that I use during my days of programming and doing tasks. At first it is confusing in many ways (because I am a confused person). It works with C and most of C's friends, one of them that I like to use is C++.</p>
		<p>Just a heads up, this is usually just for testing or for fun. If you want to make a serious or complete game, I suggest searching for guides deeper in <a href="https://github.com/search">Github</a> or other websites for more advanced tutorials.</p>
		<p>First things first, I will explain on how some things work in C++. If you don't have it, I suggest downloading it by using <a href="http://www.mingw.org/wiki/Getting_Started">MinGW</a> for Windows. On Linux, use the usual</p>
		<blockquote><p>sudo apt-get install g++</p></blockquote>
		<p>You will also need <a href="http://www.glfw.org/download.html">GLFW</a>, although you're free to use other OpenGL utilities as well.</p>
		<p>Once they have been downloaded, you will needa basic concept of object-oriented programming (or just explore C++ for yourself). There are many ways to represent a shooter game, so it is encouraged for you to find your own style afterwards.</p>
		<p align=center><img src="Shooter1.png" alt="" border="4" style="border-color: white" width = 600></p>
		<p>First of all, get started with GLFW <a href="http://www.glfw.org/docs/latest/quick.html">here</a>. Put those codes in the main program <span style="color: #00ff00;">int main()</span>. You will need at least these codes in the body:</p>
		
		<ul style="list-style-type: square;">
			<li>the glfwInit</li>
			<li>a glfwWindow (using glfwCreateWindow)</li>
			<li>select the window with glfwMakeContextCurrent</li>
			<li>a way of input, either with <a href="http://www.glfw.org/docs/latest/input_guide.html#input_keyboard">Keyboard</a> or <a href="http://www.glfw.org/docs/latest/input_guide.html#input_mouse">Mouse</a></li>
			<li>set a swap buffer of 1 with <span style="color: #00ff00;">glfwSwapInterval(1);</span></li>
			<li>the main drawing code: <span style="color: #00ff00;">while (!glfwWindowShouldClose(yourWindowName)) { }<br /></span></li>
			<li>before the end braces of the above code, add <span style="color: #00ff00;">glfwSwapBuffers(window);</span> and <span style="color: #00ff00;">glfwPollEvents();</span></li>
			<li>glfwDestroyWindow() and glfwTerminate() at the end</li>
			<li>(optional) a way to use time (glfwGetTime() usage)</li>
		</ul>
		
		<p>For basics: the initialisation codes will go before the while (!glfwWindowShouldClose()) main loop, the drawing and animation goes inside the main loop.</p>
		<h2>Drawing shapes in OpenGL</h2>
		<p>If you're fond of more advances features (which will probably be more useful than this), I highly suggest reading the well-documented <a href="http://www.opengl-tutorial.org/beginners-tutorials/tutorial-2-the-first-triangle/">OpenGL Tutorial</a> here.</p>
		<p>Drawing in OpenGL in its simplest way uses glBegin(shape_type), glVertex2f(pointx, pointy), glEnd(). For the shape_type, use GL_TRIANGLES for triangles, GL_QUADS for rectangles, GL_POLYGON for a convex shape, GL_LINE_LOOP to make the shape outline only. For squres and triangles, make sure to have 3 and 4 points or glVertex2f codes respectively. The window acts like a Cartesian grid, with point (0,0) being at the center, positive x-axis to the right and positive y-axis upwards.</p>
		<p>For example: in the main loop, enter these codes:</p>
		
		<div class="highlight"><pre class="highlight"><code>
	glBegin(GL_QUADS);
	 glVertex2f(0.5, 0.0);
	 glVertex2f(0.0, 0.5);
	 glVertex2f(-0.5, 0.0);
	 glVertex2f(0.0, -0.5);
	glEnd();
		</code></pre></div>
		<p>And it will produce the following output: </p>
		<p><img src="img1.png" alt="" border="4" style="border-color: white" width = 200 align=right>The pointx and pointy values represent the position relative to the center. For example, glVertex2f(1.0, 0.0) means that the point will be at the top and center of the screen. It is possible to draw points beyond the screen (try changing one of the vertexes to glVertext2f(2.0, 0.0)). If you want to get pixel values, get and store the window's width and length as you use glfwCreateWindow(). Alternatively, if you plan on going full screen, get window width and height from the primary monitor like this:</p>
		<div class="highlight"><pre class="highlight"><code>
	const GLFWvidmode* mode = glfwGetVideoMode(glfwGetPrimaryMonitor());
	
	HEIGHT = mode->height;
	WIDTH = mode->width;
	
	GLFWwindow *window = glfwCreateWindow(WIDTH, HEIGHT, "Title", glfwGetPrimaryMonitor(), NULL);
		</code></pre></div>
		<p>Note: Don't forget to declare HEIGHT and WIDTH as global variables (variables outside the main program).</p>
		<p>If you plan on using windowed full screen, change the fourth parameter in glfwCreateWindow() to NULL. For more details on modifying windows, check the <a href="http://www.glfw.org/docs/latest/window_guide.html">GLFW Documentation</a>. Afterwards, make a function to change values from 0 to window height, into a number between -1.0 and 1.0, the same goes for window width. You can do that with these functions:</p>
		
		<div class="highlight"><pre class="highlight"><code>
	float newWidth(float oldWidth) {
	 return 2 * oldWidth / WIDTH;
	}

	float newHeight(float oldHeight) {
	 return 2 * oldHeight / HEIGHT;
	}
		</code></pre></div>
		<p>And now, you can modify the above code to have pixel lengths. The following code will result this image:</p>
		<img src="img2.png" alt="" border="4" style="border-color: white" width = 200 align=right>
		<div class="highlight"><pre class="highlight"><code>
	glBegin(GL_QUADS);
	 glVertex2f(newWidth(100), newHeight(0.0));
	 glVertex2f(newWidth(0.0), newHeight(100));
	 glVertex2f(newWidth(-100), newHeight(0.0));
	 glVertex2f(newWidth(0.0), newHeight(-100));
	glEnd();
		</code></pre></div>
		
		<p>This method, although straightforward, is somewhat tedious as we have to insert the function everytime we need to draw a point. Another method, which is a bit easier to use, is the glDrawArrays() method. First, enter <span style="color: #00ff00;">glEnableClientState (GL_VERTEX_ARRAY);</span> before the main loop to use arrays. Then, define the array of points that you will draw, either as a global variable or somewhere else as long as it's accessible in the main loop. Next, enter glVertexPointer() to use your array. Last, simply call glDrawArrays(). The following code will illustrate the example of drawing a triangle in the main loop:</p>
		
		<img src="img3.png" alt="" border="4" style="border-color: white" width = 200 align=right>
		<div class="highlight"><pre class="highlight"><code>
	GLfloat myTriangle[3*2];
	myTriangle[0] = newWidth(100); myTriangle[1] = newHeight(0);
	myTriangle[2] = newWidth(-100); myTriangle[3] = newHeight(0);
	myTriangle[4] = newWidth(0); myTriangle[5] = newHeight(400);
	
	glVertexPointer (2, GL_FLOAT, 0, myTriangle);
	glDrawArrays(GL_TRIANGLES, 0, 3);
		</code></pre></div>
		<p>Note: Don't forget to use <span style="color: #00ff00;">glEnableClientState (GL_VERTEX_ARRAY);</span></p>
		<p>For a more complete information on these techniques, take a look at <a href="http://www.glprogramming.com/red/chapter02.html">this</a> documentation.</p>
		<p>The above code is an example but hopefully it will give you an idea on how to use it, and perhaps optimize your code with loops and functions. in glVertexPointer(), the last parameter is to be changed if you want to use another array. In glDrawArrays(), first parameter is the shape type, third parameter is the number of points you will draw, its value should be half the array size.</p>
		<p>If you want to add color, use glColor3f(<span style="color: #ff0000;">r</span>, <span style="color: #00ff00;">g</span>, <span style="color: #0000ff;">b</span>), where <span style="color: #ff0000;">r</span>, <span style="color: #00ff00;">g</span>, <span style="color: #0000ff;">b</span> stands for <span style="color: #ff0000;">red</span>, <span style="color: #00ff00;">green</span>, and <span style="color: #0000ff;">blue</span> values respectively, with colors ranging from 0.0 to 1.0. Values below 0 will be treated as 0, values above 1 will be treated as 1. If you want transparency and blending, use glColor4f(<span style="color: #ff0000;">r</span>, <span style="color: #00ff00;">g</span>, <span style="color: #0000ff;">b</span>, <span style="color: #bbbbbb;">a</span>) where the <span style="color: #aaaaaa;">a</span> determines opaqueness level (0 is invisible and 1 is opaque), and add these 2 lines of code before the main loop for transparency to work:</p>
		
		<div class="highlight"><pre class="highlight"><code>
	glEnable(GL_BLEND);
	glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
		</code></pre></div>
		
		<p>To be continued...</p>
		
	</body>
</html>
