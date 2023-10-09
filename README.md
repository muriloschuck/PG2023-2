# PG2023-2

## O que é a GLSL? Quais os dois tipos de shaders são obrigatórios no pipeline programável da versão atual que trabalhamos em aula e o que eles processam? 

### GLSL é uma linguagem C-like na qual os shaders são escritos. Um arquivo shader escrito com o GLSL é estruturado com: uma versão, uma lista de variáveis de input e output e uma função main. O OpenGL moderno necessita que utilizemos no mínimo um vertex shader e um fragment shader se quisermos fazer algum render.

## O que são primitivas gráficas? Como fazemos o armazenamento dos vértices na OpenGL?

### Para que o OpenGL saiba o que fazer com a entrada de coordenadas e cores nós precisamos indicar de que forma queremos renderizar essas informações. Essa "informação" sobre a forma é chamado de primitiva. Por exemplo, caso desejamos realizar o triângulo, podemos usar a primitiva GL_TRIANGLES. Para o OpenGL, os vértices são pontos no espaço 3D, que podem ser armazenados em um array de floats:

```
float vertices[] = {
    -0.5f, -0.5f, 0.0f,
     0.5f, -0.5f, 0.0f,
     0.0f,  0.5f, 0.0f
};  
```

## Explique o que é VBO, VAO e EBO, e como se relacionam (se achar mais fácil, pode fazer 1um gráfico representando a relação entre eles). 

### No primeiro passo da pipeline gráfica, nós utilizamos o vertex shader para alocar memória na GPU, configuramos como o OpenGL deverá interpretar essa memória e espeficifamos como enviar esse dado para a placa gráfica. Para gerenciar essa memória, usamos os Vertex Buffer Objects (VBO), no qual pode ser armazenado uma grande quantidade de vértices na memória da GPU. Já o Vertex Array Object (VAO) é utilizado como contâiner de armazenamento e organização de configurações de atributos dos vértices (como posições, cores e texturas) para uma renderização gráfica eficiente. O Element Buffer Object (EBO) é um buffer usado para especificar quais vértices devem ser renderizados. Em cenários onde temos dois triângulos que possuem vértices se sobreescrevendo:

```
float vertices[] = {
    // first triangle
     0.5f,  0.5f, 0.0f,  // top right
     0.5f, -0.5f, 0.0f,  // bottom right
    -0.5f,  0.5f, 0.0f,  // top left 
    // second triangle
     0.5f, -0.5f, 0.0f,  // bottom right
    -0.5f, -0.5f, 0.0f,  // bottom left
    -0.5f,  0.5f, 0.0f   // top left
}; 
```

### Podemos evitar o uso de processamento indicando quais indíces desenhar (em qual ordem) utilizando o EBO, sendo mais eficiente:

```
float vertices[] = {
     0.5f,  0.5f, 0.0f,  // top right
     0.5f, -0.5f, 0.0f,  // bottom right
    -0.5f, -0.5f, 0.0f,  // bottom left
    -0.5f,  0.5f, 0.0f   // top left 
};
unsigned int indices[] = {  // note that we start from 0!
    0, 1, 3,   // first triangle
    1, 2, 3    // second triangle
};  
```