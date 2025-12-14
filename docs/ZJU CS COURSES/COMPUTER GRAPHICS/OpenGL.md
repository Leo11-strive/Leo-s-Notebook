## Getting Started  
* OpenGL is a large state machine, a collection of variables that define how OpenGL should currently operate.     
  
* Object: An object in OpenGL is a collection of options that represents a subset of OpenGL's state. For example, we could have an object that represents the settings of the drawing window; we could then set its size, how many colors it supports and so on.   
  
## Create a Window  
* Configure the window  
```cpp   
int main()  
{
    glfwInit();  
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);  
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);  
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);  
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);//on mac    
    return 0;
}  
``` 
* Create the window  
```cpp  
GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);  
if (window == NULL)  
{  
    std::cout << "Failed to create GLFW window" << std::endl;  
    glfwTerminate();  
    return -1;  
}
glfwMakeContextCurrent(window);  
```  
* Load GLAD  
```cpp  
if(!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))  
{  
    std::cout << "Failed to initialize GLAD" << std::endl;  
    return -1;  
}  
``` 
* Set the viewport  
```cpp  
glViewport(0, 0, 800, 600);  
``` 
> The first two parameters of glViewport set the location of the lower left corner of the window. The third and fourth parameter set the width and height of the rendering window in pixels, which we set to 800 and 600, respectively.  
    
* Resize the window  
```cpp  
void framebuffer_size_callback(GLFWwindow* window, int width, int height)  
{  
    glViewport(0, 0, width, height);  
}
glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);  
``` 
* Render loop  
```cpp  
while(!glfwWindowShouldClose(window))  
{  
    processInput(window);  
    glfwSwapBuffers(window);  
    glfwPollEvents();  
}  
```  
> The glfwWindowShouldClose function checks at the start of each loop iteration if GLFW has been instructed to close. If so, the function returns true and the render loop stops running, after which we can close the application.  
> The glfwPollEvents function checks if any events are triggered (like keyboard input or mouse movement events), updates the window state, and calls the corresponding functions (which we can register via callback methods). The glfwSwapBuffers will swap the color buffer (a large 2D buffer that contains color values for each pixel in GLFW's window) that is used to render to during this render iteration and show it as output to the screen.  
  
* Double Buffer  
> When an application draws in a single buffer the resulting image may display flickering issues. This is because the resulting output image is not drawn in an instant, but drawn pixel by pixel and usually from left to right and top to bottom. Because this image is not displayed at an instant to the user while still being rendered to, the result may contain artifacts. To circumvent these issues, windowing applications apply a double buffer for rendering. The front buffer contains the final output image that is shown at the screen, while all the rendering commands draw to the back buffer. As soon as all the rendering commands are finished we swap the back buffer to the front buffer so the image can be displayed without still being rendered to, removing all the aforementioned artifacts.  
       
* Termination  
```cpp  
glfwTerminate();  
return 0;  
```  
  
## Input  
```cpp  
void processInput(GLFWwindow *window)  
{  
    if(glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)  
        glfwSetWindowShouldClose(window, true);  
}  
```    
> The glfwGetKey function takes the window as input together with a key. It returns whether this key is currently being pressed. If it is being pressed, the function returns GLFW_PRESS, if it is not being pressed, it returns GLFW_RELEASE.  
  
```cpp  
while (!glfwWindowShouldClose(window))  
{  
    processInput(window);  
    glfwSwapBuffers(window);  // rendering commands here  
    glfwPollEvents();
      
}  
```  
> This gives an easy way to check for specific key presses and react accordingly every frame.  
  
## Rendering  
* We want to place all the rendering commands within the render loop.  
```cpp  
while (!glfwWindowShouldClose(window))  
{  
    processInput(window);  
    glClearColor(0.2f, 0.3f, 0.3f, 1.0f);  
    glClear(GL_COLOR_BUFFER_BIT);  
    glfwSwapBuffers(window);  
    glfwPollEvents();  
}  
```  
> Just to test if things actually work we want to clear the screen with a color of our choice. At the start of frame we want to clear the screen. Otherwise we would still see the results from the previous frame (this could be the effect you're looking for, but usually you don't). We can clear the screen's color buffer using glClear where we pass in buffer bits to specify which buffer we would like to clear. The possible bits we can set are GL_COLOR_BUFFER_BIT, GL_DEPTH_BUFFER_BIT and GL_STENCIL_BUFFER_BIT. Right now we only care about the color values so we only clear the color buffer.  
  
```cpp  
glClearColor(0.2f, 0.3f, 0.3f, 1.0f);    
glClear(GL_COLOR_BUFFER_BIT);  
```  
> The glClearColor function is a state-setting function and glClear is a state-using function. The glClearColor function sets the color that is used when calling the glClear function. The glClear function is used to clear the color buffer.  
  
## Hello Triangle  
### Graphics Pipeline  
* shaders  
> written in OpenGL Shading Language (GLSL)    
!!! note "Graphics Pipeline brief illustration"

    ![alt text](<Screenshot 2025-03-07 at 17.39.44.png>)  

    
> In order for OpenGL to know what to make of your collection of coordinates and color values OpenGL requires you to hint what kind of render types you want to form with the data. Do we want the data rendered as a collection of points, a collection of triangles or perhaps just one long line? Those hints are called primitives and are given to OpenGL while calling any of the drawing commands. Some of these hints are GL_POINTS, GL_TRIANGLES and GL_LINE_STRIP where the latter hints OpenGL to render the data as a collection of points, triangles or a strip of lines respectively.  
    
### Vertex Input  
* To start drawing something we have to first give OpenGL some input vertex data. OpenGL is a 3D graphics library so all coordinates that we specify in OpenGL are in 3D (x, y and z coordinate). OpenGL doesn't simply transform all your 3D coordinates to 2D pixels on your screen; OpenGL only processes 3D coordinates when they're in a specific range between -1.0 and 1.0 on all 3 axes (x, y and z). All coordinates within this so called normalized device coordinates range will end up visible on your screen (and all coordinates outside this region won't).   

* Vertex Buffer  
> With the vertex data defined we'd like to send it as input to the first process of the graphics pipeline: the vertex shader. This is done by creating memory on the GPU where we store the vertex data, configure how OpenGL should interpret the memory and specify how to send the data to the graphics card. The vertex shader then processes as much vertices as we tell it to from its memory.

> We manage this memory via so called vertex buffer objects (VBO) that can store a large number of vertices in the GPU's memory. The advantage of using those buffer objects is that we can send large batches of data all at once to the graphics card, and keep it there if there's enough memory left, without having to send data one vertex at a time. Sending data to the graphics card from the CPU is relatively slow, so wherever we can we try to send as much data as possible at once. Once the data is in the graphics card's memory the vertex shader has almost instant access to the vertices making it extremely fast  
  
* A vertex buffer object is our first occurrence of an OpenGL object as we've discussed in the OpenGL chapter. Just like any object in OpenGL, this buffer has a unique ID corresponding to that buffer, so we can generate one with a buffer ID using the glGenBuffers function:  
```cpp  
unsigned int VBO;    
glGenBuffers(1, &VBO);  
```  
  
* bind buffer  
> After generating the buffer object we want to bind it so any subsequent buffer manipulation calls will affect this buffer. We can bind the buffer using the GL_ARRAY_BUFFER target, a type which can only signed to a particular buffer:  
```cpp
glBindBuffer(GL_ARRAY_BUFFER, VBO);  
```  
* Then we can make a call to the glBufferData function that copies the previously defined vertex data into the buffer's memory:  
```cpp  
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);  
```  
  
* **The fourth parameter has three types**  
    * GL_STATIC_DRAW: the data will most likely not change at all or very rarely, yet used many times.  
    * GL_DYNAMIC_DRAW: the data is likely to change a lot and used many times.  
    * GL_STREAM_DRAW: the data is set only once and used by the GPU at most a few times. 

    
  
## Vertex Shader  
```cpp  
#version 330 core  
layout (location = 0) in vec3 aPos;  
void main()  
{  
    gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);  
}  
```  
* The first line specifies the version of GLSL we're using (in this case, version 3.3).  
* The second line specifies the input of the vertex shader. The vertex shader receives its input straight from the vertex data so we have to specify what kind of input we're feeding it. We do this by declaring the input variables with the in keyword. The location specifier is an input layout qualifier that specifies at what location the input variable is in the vertex data.  
* The main function is the entry point of the shader that gets called for each vertex we specify. The vertex shader should set gl_Position to a vec4 that specifies the position of the vertex.  

  

    
## Compile a shader  
```cpp  
const char *vertexShaderSource = "#version 330 core\n"  
    "layout (location = 0) in vec3 aPos;\n"  
    "void main()\n"  
    "{\n"  
    "   gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);\n"  
    "}\0";  
```  
* we store the vertex shader source code in a C string.  
* In order for OpenGL to use the shader it has to dynamically compile it at run-time from its source code. The first thing we need to do is create a shader object, again referenced by an ID. So we store the vertex shader as an unsigned int and create the shader with glCreateShader:  
```cpp  
unsigned int vertexShader;  
vertexShader = glCreateShader(GL_VERTEX_SHADER);  
```  
> We provide the type of shader we want to create as an argument to glCreateShader. Since we're creating a vertex shader we pass in GL_VERTEX_SHADER.  
```cpp  
glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);  
glCompileShader(vertexShader);  
```  
> The glShaderSource function takes the shader object to compile to as its first argument. The second argument specifies how many strings we're passing as source code, which is only one. The third parameter is the actual source code of the vertex shader and we can leave the 4th parameter to NULL.      
  
* Check if the shader compiled successfully  
```cpp  
int success;  
char infoLog[512];  
glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success);  
if(!success)  
{  
    glGetShaderInfoLog(vertexShader, 512, NULL, infoLog);  
    std::cout << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n" << infoLog << std::endl;  
}  
```  
> We can retrieve the compile errors with the glGetShaderiv function that retrieves the compile status of the shader. We pass in the shader object and the status type we want to retrieve. If the compilation failed we can retrieve the error message with the glGetShaderInfoLog function. We pass in the shader object, the size of the buffer the error message should be stored in, a pointer to a variable that will store the length of the error message and a buffer that will store the error message.  
  
## Fragment shader  
* Colors in computer graphics are represented as an array of 4 values: the red, green, blue and alpha (opacity) component, commonly abbreviated to RGBA. When defining a color in OpenGL or GLSL we set the strength of each component to a value between 0.0 and 1.0. If, for example, we would set red to 1.0 and green to 1.0 we would get a mixture of both colors and get the color yellow. Given those 3 color components we can generate over 16 million different colors!  
```cpp  
const char *fragmentShaderSource = "#version 330 core\n"  
    "out vec4 FragColor;\n"  
    "void main()\n"  
    "{\n"  
    "   FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);\n"  
    "}\n\0";  
```  
* The out keyword is used for defining output variables.  
* The vec4 type is a 4-component vector that we use to represent the color.  
* The main function of the fragment shader is quite simple: it sets the output color to orange with full opacity.  
```cpp  
unsigned int fragmentShader;  
fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);  
glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);  
glCompileShader(fragmentShader);  
```  
  
## Shader Program  
```cpp  
unsigned int shaderProgram;  
shaderProgram = glCreateProgram();  
glAttachShader(shaderProgram, vertexShader);  
glAttachShader(shaderProgram, fragmentShader);  
glLinkProgram(shaderProgram);  
```  
* to check if the linking was successful  
```cpp  
int success;
char infoLog[512];
glGetProgramiv(shaderProgram, GL_LINK_STATUS, &success);
if(!success)
{
    glGetProgramInfoLog(shaderProgram, 512, NULL, infoLog);
    std::cout << "ERROR::SHADER::PROGRAM::LINKING_FAILED\n" << infoLog << std::endl;
}
```  
* activate the shader program  
```cpp
glUseProgram(shaderProgram);
```  
* delete the shader objects once we've linked them into a program; we no longer need them.  
```cpp
glDeleteShader(vertexShader);
glDeleteShader(fragmentShader);
```    
  
## Linking Vertex Attributes  
* The vertex shader allows us to specify any input we want in the form of vertex attributes and while this allows for great flexibility, it does mean we have to manually specify what part of our input data goes to which vertex attribute in the vertex shader. This means we have to specify how OpenGL should interpret the vertex data before rendering.  
  
```cpp  
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
```  
* Each vertex attribute takes its data from memory managed by a VBO and which VBO it takes its data from (you can have multiple VBOs) is determined by the VBO currently bound to GL_ARRAY_BUFFER when calling glVertexAttribPointer. Since the previously defined VBO is still bound before calling glVertexAttribPointer vertex attribute 0 is now associated with its vertex data!  
  
* drawing an object is something like this  
```cpp  
// 0. copy our vertices array in a buffer for OpenGL to use
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
// 1. then set the vertex attributes pointers
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);  
// 2. use our shader program when we want to render an object
glUseProgram(shaderProgram);
// 3. now draw the object 
someOpenGLFunctionThatDrawsOurTriangle();  
```    
    
-----------------------------------------------------  

## Vertex Array Object    
=== "create a VAO object"  
```cpp  
unsigned int VAO;  
glGenVertexArrays(1, &VAO);   
```     
=== "bind the VAO"  
```cpp
glBindVertexArray(VAO);  
```   
    
      
        
          
  

## Draw triangle  
```cpp      
glUseProgram(shaderProgram);
glBindVertexArray(VAO);  
glDrawArrays(GL_TRIANGLES, 0, 3);  
```    
  
## Element Buffer Objects  
```cpp  
float vertices[] = {  
    0.5f,  0.5f, 0.0f,  // top right  
    0.5f, -0.5f, 0.0f,  // bottom right  
    -0.5f, -0.5f, 0.0f,  // bottom left  
    -0.5f,  0.5f, 0.0f   // top left  
};  
unsigned int indices[] = {  
    0, 1, 3,  // first triangle  
    1, 2, 3   // second triangle  
};  
```  
* create a new buffer object for the indices  
```cpp  
unsigned int EBO;  
glGenBuffers(1, &EBO);  
```    
  
* bind the EBO  
```cpp  
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);  
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);  
```  
  
* draw the object  
```cpp  
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);  
```    

* The last parameter of glDrawElements is of type void* and thus requires a cast. This parameter is called the offset of the EBO and is currently set to 0.
  
## Draw a rectangle    
```cpp  
// ..:: Initialization code :: ..
// 1. bind Vertex Array Object
glBindVertexArray(VAO);
// 2. copy our vertices array in a vertex buffer for OpenGL to use
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
// 3. copy our index array in a element buffer for OpenGL to use
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
// 4. then set the vertex attributes pointers
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);  

[...]
  
// ..:: Drawing code (in render loop) :: ..
glUseProgram(shaderProgram);
glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
glBindVertexArray(0);  
```  
  
* wireframe mode  
```cpp
glPolygonMode(GL_FRONT_AND_BACK, GL_LINE);  
```  
```cpp  
glPolygonMode(GL_FRONT_AND_BACK, GL_FILL);  
```  
  
## Shaders   

* Shaders always begin with a version declaration, followed by a list of input and output variables, uniforms and its main function. Each shader's entry point is at its main function where we process any input variables and output the results in its output variables.   

``` cpp    
#version version_number
in type in_variable_name;
in type in_variable_name;

out type out_variable_name;
  
uniform type uniform_name;
  
void main()
{
  // process input(s) and do some weird graphics stuff
  ...
  // output processed stuff to output variable
  out_variable_name = weird_stuff_we_processed;
}  
```  
  
* **vertex attributes**:  
   
* When we're talking specifically about the vertex shader each input variable is also known as a vertex attribute. There is a maximum number of vertex attributes we're allowed to declare limited by the hardware. OpenGL guarantees there are always at least 16 4-component vertex attributes available, but some hardware may allow for more which you can retrieve by querying GL_MAX_VERTEX_ATTRIBS  

```cpp    
int nrAttributes;
glGetIntegerv(GL_MAX_VERTEX_ATTRIBS, &nrAttributes);
std::cout << "Maximum nr of vertex attributes supported: " << nrAttributes << std::endl;  
```  
  
### Types  

* GLSL has most of the default basic types we know from languages like C: int, float, double, uint and bool.  
* vectors and matrices  
  
### Vectors   

* A vector in GLSL is a 2,3 or 4 component container for any of the basic types just mentioned.    

> * vecn: the default vector of n floats.
> * bvecn: a vector of n booleans.
> * ivecn: a vector of n integers.
> * uvecn: a vector of n unsigned integers.
> * dvecn: a vector of n double components.  
  
* Components of a vector can be accessed via vec.x where x is the first component of the vector. You can use .x, .y, .z and .w to access their first, second, third and fourth component respectively. GLSL also allows you to use rgba for colors or stpq for texture coordinates, accessing the same components.   

* **swizzling**   

```cpp  
vec2 someVec;  
vec4 differentVec = someVec.xyxx;  
vec3 anotherVec = differentVec.zyw;    
vec4 otherVec = someVec.xxxx + anotherVec.yxzy;    
```  

``` cpp  
vec2 vect = vec2(0.5, 0.7);
vec4 result = vec4(vect, 0.0, 0.0);
vec4 otherResult = vec4(result.xyz, 1.0);  
```  

### Ins and Outs  

* Shaders are nice little programs on their own, but they are part of a whole and for that reason we want to have inputs and outputs on the individual shaders so that we can move stuff around. GLSL defined the in and out keywords specifically for that purpose. Each shader can specify inputs and outputs using those keywords and wherever an output variable matches with an input variable of the next shader stage they're passed along.  


* *Uniform*:

> A uniform is a global variable that is declared with the uniform keyword. Uniforms are used to pass data from the CPU to the GPU and are read-only in the shader. They are set once per draw call and can be used to pass data such as transformation matrices, colors, or any other data that needs to be consistent across all vertices or fragments.  

```cpp
float timeValue = glfwGetTime();
float greenValue = (sin(timeValue) / 2.0f) + 0.5f;
int vertexColorLocation = glGetUniformLocation(shaderProgram, "ourColor");
glUseProgram(shaderProgram);
glUniform4f(vertexColorLocation, 0.0f, greenValue, 0.0f, 1.0f);
```

*Note that finding the uniform location does not require you to use the shader program first, but updating a uniform does require you to first use the program (by calling glUseProgram), because it sets the uniform on the currently active shader program.*

```cpp
while(!glfwWindowShouldClose(window))
{
    // input
    processInput(window);

    // render
    // clear the colorbuffer
    glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
    glClear(GL_COLOR_BUFFER_BIT);

    // be sure to activate the shader
    glUseProgram(shaderProgram);
  
    // update the uniform color
    float timeValue = glfwGetTime();
    float greenValue = sin(timeValue) / 2.0f + 0.5f;
    int vertexColorLocation = glGetUniformLocation(shaderProgram, "ourColor");
    glUniform4f(vertexColorLocation, 0.0f, greenValue, 0.0f, 1.0f);

    // now render the triangle
    glBindVertexArray(VAO);
    glDrawArrays(GL_TRIANGLES, 0, 3);
  
    // swap buffers and poll IO events
    glfwSwapBuffers(window);
    glfwPollEvents();
}
```












