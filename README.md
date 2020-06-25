# Como ler arquivo texto em Java (aplicação real) - Aulão #002
###### DevSuperior - sua carreira dev com fundamento de ensino superior

[![Image](https://s3-sa-east-1.amazonaws.com/educandoweb.com.br/img/devsuperior/bt-youtube.png "DevSuperior no Youtube")](https://youtube.com/devsuperior) [![Image](https://s3-sa-east-1.amazonaws.com/educandoweb.com.br/img/devsuperior/bt-facebook.png "DevSuperior no Facebook")](https://facebook.com/devsuperior.fb) [![Image](https://s3-sa-east-1.amazonaws.com/educandoweb.com.br/img/devsuperior/bt-instagram.png "DevSuperior no Instagram")](https://instagram.com/devsuperior.ig)

Assista o vídeo:

[![Image](https://img.youtube.com/vi/xLDViuYlqGM/mqdefault.jpg "Vídeo no Youtube")](https://youtu.be/xLDViuYlqGM)

## Sumário
- [O que você vai aprender](#O-que-você-vai-aprender)
- [Pré-requisitos](#pré-requisitos)
- [Conteúdo do arquivo CSV](#Conteúdo-do-arquivo-CSV)
- [Código fonte em etapas](#Código-fonte-em-etapas)
  - [Etapa 1: Leitura simples com File e Scanner](#Etapa-1-Leitura-simples-com-File-e-Scanner)
  - [Etapa 2: FileReader e BufferedReader com controle manual](#Etapa-2-FileReader-e-BufferedReader-com-controle-manual)
  - [Etapa 3: Usando bloco 'try with resources'](#Etapa-3-Usando-bloco-try-with-resources)
  - [Etapa 4: lendo arquivo CSV e instanciando uma lista de objetos](#Etapa-4-lendo-arquivo-CSV-e-instanciando-uma-lista-de-objetos)

## O que você vai aprender
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

## Conteúdo do arquivo CSV

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

### Etapa 1: Leitura simples com File e Scanner

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

### Etapa 2: FileReader e BufferedReader com controle manual

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

### Etapa 3: Usando bloco 'try with resources'

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
### Etapa 4: lendo arquivo CSV e instanciando uma lista de objetos

```java
package application;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import entities.Product;

public class Program {

	public static void main(String[] args) {

		String path = "c:\\temp\\in.txt";
		
		List<Product> list = new ArrayList<Product>();
		
		try (BufferedReader br = new BufferedReader(new FileReader(path))) {
			
			String line = br.readLine();
			line = br.readLine();
			while (line != null) {
				
				String[] vect = line.split(",");
				String name = vect[0];
				Double price = Double.parseDouble(vect[1]);
				Integer qte = Integer.parseInt(vect[2]);
				
				Product prod = new Product(name, price, qte);
				list.add(prod);
				
				line = br.readLine();
			}	
			
			System.out.println("PRODUCTS:");
			for (Product p : list) {
				System.out.println(p);
			}
		}
		catch (IOException e) {
			System.out.println("Error: " + e.getMessage());
		}
	}
}
```

```java
package entities;

import java.io.Serializable;

public class Product implements Serializable {
	private static final long serialVersionUID = 1L;

	private String name;
	private Double price;
	private Integer quantity;
	
	public Product() {
	}

	public Product(String name, Double price, Integer quantity) {
		super();
		this.name = name;
		this.price = price;
		this.quantity = quantity;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Double getPrice() {
		return price;
	}

	public void setPrice(Double price) {
		this.price = price;
	}

	public Integer getQuantity() {
		return quantity;
	}

	public void setQuantity(Integer quantity) {
		this.quantity = quantity;
	}

	@Override
	public String toString() {
		return "Product [name=" + name + ", price=" + price + ", quantity=" + quantity + "]";
	}
}
```


