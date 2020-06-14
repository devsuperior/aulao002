# Como ler arquivo texto em Java (aplicação real) - Aulão #002

DevSuperior - Prof. Dr. Nelio Alves

Siga-nos:

[![Image](https://s3-sa-east-1.amazonaws.com/educandoweb.com.br/img/devsuperior/bt-youtube.png "DevSuperior no Youtube")](https://www.youtube.com/channel/UC3twHmWQwtqEO7u-gB_2f7g) [![Image](https://s3-sa-east-1.amazonaws.com/educandoweb.com.br/img/devsuperior/bt-facebook.png "DevSuperior no Facebook")](https://www.youtube.com/channel/UC3twHmWQwtqEO7u-gB_2f7g) [![Image](https://s3-sa-east-1.amazonaws.com/educandoweb.com.br/img/devsuperior/bt-instagram.png "DevSuperior no Instagram")](https://www.youtube.com/channel/UC3twHmWQwtqEO7u-gB_2f7g)

Assista o vídeo:

[![Image](https://s3-sa-east-1.amazonaws.com/educandoweb.com.br/img/devsuperior/capa005-small.jpg "Vídeo no Youtube")](https://www.youtube.com/channel/UC3twHmWQwtqEO7u-gB_2f7g)

## O que você vai aprender?
- Classes File, Scanner, IOException
- Classes FileReader, BufferedReader
- Block try-catch-finally para controle manual dos recursos
- Bloco "try with resources" para controle automatizado dos recursos
- Como ler um arquivo .CSV e transformá-lo em uma lista de objetos em memória

## Pré-requisitos

- Lógica de programação
  - Variáveis, entrada, processamento, saída
  - Estrutura condicional
  - Estruturas repetitivas
- OOP básica
  - Classes, atributos, métodos, objetos
  - Construtores, encapsulamento
  - Coleções (List)

## Conteúdo do arquivo .CSV

```
Name,Price,Quantity
Notebook Gamer,2100.90,10
Smartphone X,1890.00,23
TV LED 70,3500.89,8
```

```
Name;Price;Quantity
Notebook Gamer;2100,90;10
Smartphone X;1890,00;23
TV LED 70;3500,89;8
```

## Código fonte em etapas

#### Etapa 1: leitura simples com File e Scanner

```java
package application;

import java.io.File;
import java.io.IOException;
import java.util.Scanner;

public class Program {

	public static void main(String[] args) {

		File file = new File("C:\\temp\\in.txt");
		Scanner sc = null;
		try {
			sc = new Scanner(file);
			while (sc.hasNextLine()) {
				System.out.println(sc.nextLine());
			}
		} 
		catch (IOException e) {
			System.out.println("Error: " + e.getMessage());
		} 
		finally {
			if (sc != null) {
				sc.close();
			}
		}
	}
}
```

#### Etapa 2: FileReader e BufferedReader com controle manual

```java
package application;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Program {

	public static void main(String[] args) {

		String path = "C:\\temp\\in.txt";
		BufferedReader br = null;
		FileReader fr = null;

		try {
			fr = new FileReader(path);
			br = new BufferedReader(fr);

			String line = br.readLine();

			while (line != null) {
				System.out.println(line);
				line = br.readLine();
			}
		} 
		catch (IOException e) {
			System.out.println("Error: " + e.getMessage());
		} 
		finally {
			try {
				if (br != null)
					br.close();
				if (fr != null)
					fr.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```

#### Etapa 3: Usando bloco "try with resources"

```java
package application;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Program {

	public static void main(String[] args) {

		String path = "C:\\temp\\in.txt";

		try (BufferedReader br = new BufferedReader(new FileReader(path))) {

			String line = br.readLine();
			while (line != null) {
				System.out.println(line);
				line = br.readLine();
			}

		} 
		catch (IOException e) {
			System.out.println("Error: " + e.getMessage());
		}
	}
}
```
