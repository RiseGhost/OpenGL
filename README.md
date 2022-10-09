# OpenGL quick guide:

<p align="center">
<img src="https://cdn.freebiesupply.com/logos/large/2x/opengl-1-logo-png-transparent.png">
</p>

Este quick guide irá utilizar bibliotecas de C++ como por exemplo:
- GLFW
- glm
- common

## Window creation 🪟🔨:
Para criar uma janela utilizamos a função __glfwCreateWindow()__. Essa função recebe 4 argumentos:
- int Width
- int Height
- const char* title
- GLFWmonitor* monitor
- GLFWwindow* share

No entanto o GLFWmomitor* monitor e o GLFWwindow* share iram ser tratados mais a frente no documento. Para já vamos dar NULL para os dois.

Então para criar uma janela com 640 x 480 com "My Window" como titulo basta fazer:

```C++
GLFWwindow* window = glfwCreateWindow(640, 480, "My Window", NULL, NULL)
```

## Get user monitor data 🖥 ℹ️:
### Number of monitors:
Para obter o número de monitores que o usuário tem podemos usar a função __GLFWmonitor** glfwGetMonitors(int* count)__. Recebe como argumento um apontador para um inteiro.
```C++
int count;
GLFWmonitor** monitors = glfwGetMonitors(&count);
```

O número de monitores fica armazenado na variável count. Basta fazer um print para obter o números de monitores que o utilizador têm.

### Monitor resolution and framerate:
Para obter a resolução do monitor do usuário vamos primeiro usar a função __GLFWmonitor* glfwGetPrimaryMonitor(void)__. Essa função retorna para nós a monitor primário do utilizador.
Depois passamos o output dessa função para a __glfwGetVideoMode__ que nos vai devolver uma struct com algumas propriedades do monitor.

```C++
GLFWmonitor* monitor = glfwGetPrimaryMonitor();
const GLFWvidmode* mode = glfwGetVideoMode(monitor);

fprintf(stdout, "width -> %i\n", mode->width);
fprintf(stdout, "height -> %i\n", mode->height);
fprintf(stdout, "framerate -> %i\n", mode->refreshRate);
```

### Physical size 📐:
Utilizando a função __glfwGetMonitorPhysicalSize()__, conseguimos fazer isso que maneira fácil. Ela recebe 3 argumentos:
- GLFWmonitor * monitor,
- int* width
- int* height

```C++
int width_mm, height_mm;
glfwGetMonitorPhysicalSize(glfwGetPrimaryMonitor(), &width_mm, &height_mm);
fprintf(stdout, "width -> %i height -> %i\n", width_mm, height_mm);
```
Os valores do comprimento e da altura ficam guardados nas váriaveis, wifth_mm e height_mm, repetivamente em milimetros.

## Create full screen window 🪟:
Para criar uma janela em tela cheia vamos utilizadar as informações do monitor.

```C++
GLFWmonitor* monitor = glfwGetPrimaryMonitor();
const GLFWvidmode* mode = glfwGetVideoMode(monitor);

GLFWwindow* window = glfwCreateWindow(mode->width, mode->height, "Full screen Window", monitor, NULL)
```

O que estamos a fazer é criar uma janela em full screen com o tamanho do ecrã 🖥 do nosso utilizador.  A resolução pode ser diferente da do ecrã do utilizador, o que acontece é se for menor a tela aparece com barras pretas a volta se for maior simplesmente sai da tela.

## OpenGL background:
### Change Background Color:
Para poder alterar a cor de fundo basta utilizar a função __glClearColor()__. Ela recebe 4 argumentos do tipo float entre 1.0 e 0.0. Onde 1.0 seria o 255 e o 0.0 seria o 0. O ultimo argumento é o alpha channel, responsável pelo opacidade.

```C++
glClearColor(1.0f, 1.0f, 1.0f, 1.0f);
```

Neste caso estamos a colocar a cor de fundo branca com a opacidade no máximo.
### Clear background:
Para limpar o background pordemos utilizar a função __glClear()__. Ela recebe apenas um argumento que é um color buffer.

```C++
glClear(GL_COLOR_BUFFER_BIT);
```

## Run the Window:
O código a baixo é capaz de executar uma janela de 640 x 480 com o fundo todo branco.

```C++
#include <GLFW/glfw3.h>

int main(void)
{
    GLFWwindow* window;

    /* Initialize the library */
    glfwInit();

    /* Create a windowed mode window and its OpenGL context */
    window = glfwCreateWindow(640, 480, "Hello World", NULL, NULL);
    if (!window)
    {
        glfwTerminate();
        return -1;
    }

    /* Make the window's context current */
    glfwMakeContextCurrent(window);

    /* Loop until the user closes the window */
    while (!glfwWindowShouldClose(window))
    {
        /* Window Background Color:*/
        glClearColor(1.0f, 1.0f, 1.0f, 1.0f);
        /* Render here */
        glClear(GL_COLOR_BUFFER_BIT);

        /* Swap front and back buffers */
        glfwSwapBuffers(window);

        /* Poll for and process events */
        glfwPollEvents();
    }

    glfwTerminate();
    return 0;
}
```

## OpenGL glfwWindowHint table options:
Na tabela a baixo estão precentes algumas funções que são possiveis serem feitas utilizando o __glfwWindowHint()__:
<table style="width:100%">
	<tr valign="middle">
		<th>Command</th>
		<th>Func</th>
	</tr>
	<tr>
		<td>GLFW_SAMPLES</td>
		<td>Ativar anti-aliasing (MSAA).O 2º argumento é para definir o nivel do anti-aliasing</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GFLW_SAMPLES, 4)
		</td>
	</tr>
	<tr>
		<td>GLFW_FOCUSED</td>
		<td>Premite focar a janela.</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GFLW_FOCUSED, 0)
		</td>
	</tr>
	<tr>
		<td>GLFW_RESIZABLE</td>
		<td>Premetir que o utilizador possa alterar o tamanho da janela. Recebe como 2º argumento, GL_TRUE ou GL_FALSE.</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GFLW_RESIZABLE, GL_TRUE)
		</td>
	</tr>
	<tr>
		<td>GLFW_MAXIMIZED</td>
		<td>Premite com que a aplicação inicie maximizada. O 2º argumento pode ser: GLFW_FALSE ou GLFW_TRUE.</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GFLW_MAXIMIZED, GLFW_FALSE)
		</td>
	</tr>
	<tr>
		<td>GLFW_TRANSPARENT_FRAMEBUFFER</td>
		<td>Premite ativar o canal alpha e criar imagens com opacidade. O 2º argumento pode ser: GLFW_TRUE ou GLFW_FALSE</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GFLW_TRANSPARENT_FRAMEBUFFER, GLFW_TRUE)
		</td>
	</tr>
	<tr>
		<td>GLFW_VISIBLE</td>
		<td>Premite mostrar ou ocultar a janela. O 2º argumento pode ser: GLFW_FALSE ou GLFW_TRUE</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GFLW_VISUBLE, GLFW_FALSE)
		</td>
	</tr>
	<tr>
		<td>GLFW_FLOATING</td>
		<td>Especifica se a janela estará flutuando ("floating") a cima das outras. O 2º argumento pode ser: GLFW_FALSE ou GLFW_TRUE. Se tiver true a janela vai ficar por cima de qualquer outra janela.</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GLFW_FLOATING, GFLW_TRUE)
		</td>
	</tr>
	<tr>
		<td>GLFW_CENTER_CURSOR</td>
		<td>Premite centar o ponteiro no centro na tela. Não funciona se o programa estiver em modo janela. O 2º argumento pode ser: GLFW_TRUE ou GLFW_FALSE</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GLFW_CENTER_CURSOR, GFLW_FALSE)
		</td>
	</tr>
	<tr>
		<td>GLFW_DECORATED</td>
		<td>Permite altera a decoração de borda e Widgets da janela. Basicamente se estiver False a janela fica sem aquela borda do windows que tem os botões de fechar, minimizar e maximizar. O 2º argumento pode ser: GLFW_FALSE ou GLFW_TRUE.</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GLFW_DECORATED, GFLW_FALSE)
		</td>
	</tr>
	<tr>
		<td>GLFW_CONTEXT_VERSION_MAJOR</td>
		<td>Indica qual a ultima versão da API que o software suporta. O 2º argumento é a versão</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GFLW_CONTEXT_VERSION_MAJOR, 3)
		</td>
	</tr>
	<tr>
		<td>GLFW_CONTEXT_VERSION_MINOR</td>
		<td>Indica qual a versão mais antiga da API que o software suporta. O 2º argumento é a versão</td>
	</tr>
	<tr style="text-align:center">
		<td colspan="2">
			glfwWindowHint(GFLW_CONTEXT_VERSION_MINOR, 2)
		</td>
	</tr>
<table>

## OpenGL Viewport (janela de visualização) 👁️:
O __Viewport__ serve para informar ao OpenGL o tamanho da janela de renderização.

```C++
glViewport(int x,int y,int Width,int Height)
```

<table>
<tr>
<td>

```C++
glViewport(0, 0, WindowWidth, WindowHeight);
```

</td>
<td>

```C++
glViewport(100, 100, 420, 420);             
```

</td>
</tr>
<tr>
	<td><img src="https://user-images.githubusercontent.com/91985039/194772299-4c2e2cac-e997-48b0-8dcc-e078f9e6d5e1.jpg"></td>
	<td><img src="https://user-images.githubusercontent.com/91985039/194772298-a8ac5dc1-4a53-424f-b83a-2748099e9fb0.jpg"></td>
</tr>
</table>

## Ajustar a rendeziração sempre que o tamanho da janela variar 📐🪟:
No momento em que utilizador redimensina a janela, a janela de visualização també deve ser ajustada. Podemos registar uma função de retorno de chamada na janela que é chamada toda a vez que a janela é redimensionada. Esta função de callback tem o seguinte protótipo:

```C++
void AjustWindow(GLFWwindow* window, int width, int height)
```

Sempre que a janela muda de tamanho o __GLFW__ chama esta função e preenche os argumentos.

```C++
void AjustWindow(GLFWwindow* window, int width, int height) {
	glViewport(0, 0, width, height);
}

int main(void){
	// Initialise GLFW
	glfwInit();

	GLFWwindow* window = glfwCreateWindow(800, 800, "AjustWindow ", NULL, NULL);
	glfwSetFramebufferSizeCallback(window, AjustWindow);
}
```
O code block de cima encontra-se simplificado.

<table>
<tr>
<th colspan="2">Exemplo:</th>
</tr>
<tr>
<td>
<img src="https://user-images.githubusercontent.com/91985039/194774260-11fab047-f09d-492c-bae8-71371072f38c.jpg">
</td>
<td>
<img src="https://user-images.githubusercontent.com/91985039/194774267-469f29d7-bae2-45cc-a484-cf6177e598bc.jpg">
</td>
</tr>
</table>
