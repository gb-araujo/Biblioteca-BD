<h1 align="center"> Biblioteca </h1>
<h2 align="center">  O Faxineiro Implacável </h2>
<h3 align="center">  Atividade de banco de dados utilizando MySQL. </h3> 
<p align="center"><img src="https://static-00.iconduck.com/assets.00/database-mysql-icon-462x512-6itsq0zm.png" width= 80px; /></p>
<hr>

Nesta atividade, foram realizadas várias etapas para reorganizar e normalizar um banco de dados chamado Biblioteca, que armazena informações sobre livros. Explicação de cada etapa:
<h2> 1 - Etapa</h2>
Adicionando a regra AUTO_INCREMENT para a chave primária e removendo dados referentes ao ID dos livros:
```
CREATE TABLE IF NOT EXISTS Livros (
    livros_id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255),
	autor VARCHAR(255),
    editora VARCHAR(255),
    ano_publicacao INT,
    isbn VARCHAR(13)
);
```

A primeira ação realizada foi adicionar a regra AUTO_INCREMENT para a coluna 'livros_id', que é a chave primária da tabela 'Livros'. Essa regra faz com que o valor da chave primária seja incrementado automaticamente a cada novo registro inserido, evitando a necessidade de fornecer um valor manualmente. Além disso, os valores relacionados ao ID dos livros presentes no script de inserção foram removidos.
<hr>
<h2> 2 - Etapa</h2>
Criando tabelas para 'Autores' e 'Editoras':
Duas novas tabelas foram criadas: 'Autores' e 'Editoras'. Essa ação teve como objetivo separar as informações sobre autores e editoras dos livros, evitando repetições e redundâncias nos dados. Cada tabela possui uma chave primária para gerar relacionamentos com a tabela 'Livros'.

```
CREATE TABLE IF NOT EXISTS Autores (
    autor_id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(255)
);

CREATE TABLE IF NOT EXISTS Editoras (
    editora_id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(255)
);
```

<hr>
<h2> 3 - Etapa</h2>
Utilizando ALTER TABLE para eliminar colunas e adicionar chaves estrangeiras:
Agora, utilizando o comando ALTER TABLE, foram realizadas duas ações na tabela 'Livros'. Primeiro, as colunas 'autor' e 'editora' foram removidas, pois essas informações foram movidas para as novas tabelas 'Autores' e 'Editoras'. Em seguida, foram adicionadas as colunas 'autor_id' e 'editora_id' na tabela 'Livros' para fazer referência às chaves primárias das tabelas 'Autores' e 'Editoras', respectivamente. Essas colunas são as chaves estrangeiras que estabelecem o relacionamento entre as tabelas.

```
ALTER TABLE Livros DROP COLUMN autor, DROP COLUMN editora;

ALTER TABLE Livros ADD COLUMN autor_id INT, ADD COLUMN editora_id INT;

ALTER TABLE Livros ADD CONSTRAINT FK_Livros_Autores FOREIGN KEY (autor_id) REFERENCES Autores(autor_id),
ADD CONSTRAINT FK_Livros_Editoras FOREIGN KEY (editora_id) REFERENCES Editoras(editora_id);
```

<hr> 
<h2> 4 - Etapa</h2>
Retirando os valores de autores e editoras do script inicial e inserindo-os nas novas tabelas:
Os valores referentes aos autores e editoras presentes no script inicial foram removidos, pois agora essas informações serão armazenadas nas tabelas 'Autores' e 'Editoras'. A tabela 'Livros' passou a armazenar apenas as chaves estrangeiras 'autor_id' e 'editora_id', que fazem referência aos registros correspondentes nas novas tabelas.

```
INSERT INTO Autores (nome) VALUES 
('John Green'),
('J.K. Rowling'),
('J.R.R. Tolkien'),
('J.D. Salinger'),
('George Orwell'),
('Rick Riordan'),
('João Guimarães Rosa'),
('Machado de Assis'),
('Graciliano Ramos'),
('Aluísio Azevedo'),
('Mário de Andrade');

INSERT INTO Editoras (nome) VALUES 
('Intrínseca'),
('Rocco'),
('Martins Fontes'),
('Little, Brown and Company'),
('Companhia Editora Nacional'),
('Nova Fronteira'),
('Companhia das Letras'),
('Martin Claret'),
('Penguin Companhia');
```

<hr>
Após realizar essas etapas de reestruturação do banco de dados, foi necessário ajustar o script que adicionava novos livros à biblioteca. O código original continha dados que não estavam de acordo com a nova estrutura do banco de dados. Então, foram feitas modificações no script para que os valores fossem inseridos corretamente, utilizando as chaves estrangeiras 'autor_id' e 'editora_id' para relacionar os livros com seus respectivos autores e editoras.

```
INSERT INTO Livros (titulo, ano_publicacao, isbn, autor_id, editora_id) VALUES
('A Culpa é das Estrelas', 2012, '9788580573466', 1, 1),
('Harry Potter e a Pedra Filosofal', 1997, '9788532511010', 2, 2),
('O Senhor dos Anéis: A Sociedade do Anel', 1954, '9788533603149', 3, 3),
('The Catcher in the Rye', '1951', '9780316769488', 4, 4),
('1984', 1949, '9788522106169', 5, 5),
('Percy Jackson e o Ladrão de Raios', 2005, '9788598078355', 6, 1),
('Grande Sertão: Veredas', 1956, '9788520923251', 7, 6),
('Memórias Póstumas de Brás Cubas', 1881, '9788535910663', 8, 7),
('Vidas Secas', 1938, '9788572326972', 9, 5),
('O Alienista', 1882, '9788572327429', 8, 8),
('O Cortiço', 1890, '9788579027048', 10, 9),
('Dom Casmurro', 1899, '9788583862093', 8, 9),
('Macunaíma', 1928, '9788503012302', 11, 5);

SELECT * from Livros;
```

Essas alterações foram necessárias para normalizar e higienizar a base de dados, evitando redundâncias, inconsistências e melhorando a eficiência e organização do sistema de informações sobre os livros da biblioteca.
