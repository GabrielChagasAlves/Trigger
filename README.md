# Trigger

## Nesta tarefa iremos reproduzir os exercicios apresentados em aula mostrando a criação das etapas e os resultados obtidos 

### 1) REPRODUZA O PRIMEIRO CÓDIGO SUGERIDO NO MYSQL WORKBENCH;
#### Criação das tabelas 
```SQL

CREATE TABLE Pedidos (
    IDPedido INT AUTO_INCREMENT PRIMARY KEY,
    DataPedido DATETIME,
    NomeCliente VARCHAR(100)
);

INSERT INTO Pedidos (DataPedido, NomeCliente) VALUES
    (NOW(), 'Josefa'),
    (NOW(), 'Carlos'),
    (NOW(), 'Joana');

select * from pedidos;
```
#### Criação do Trigger
```SQL
DELIMITER $
CREATE TRIGGER RegistroDataCriacaoPedido
BEFORE INSERT ON Pedidos
FOR EACH ROW 
BEGIN 
		SET NEW.DataPedido = NOW();
END;
$
DELIMITER ;
```
### 2) EXECUTE AS ETAPAS E VERIFIQUE SEUS RESULTADOS;
#### Teste do Trigger 
```SQL
INSERT INTO Pedidos (NomeCliente) VALUES ('Novo Cliente');
```
#### Resultados 
```SQL
SELECT * FROM Pedidos;
```

### 3) APÓS A EXECUÇÃO DO PRIMEIRO CÓDIGO REALIZE O SEGUNDO EXEMPLO;
```SQL
CREATE TABLE Filmes(
id INT PRIMARY KEY AUTO_INCREMENT,
titulo VARCHAR (60),
minutos INT);

DELIMITER $

CREATE TRIGGER chk_minutos BEFORE INSERT ON Filmes 
	FOR EACH ROW 
    BEGIN 
		IF NEW.minutos < 0 THEN 
			SET NEW.minutos = NULL;
		END IF;
	END$
    
    DELIMITER ;
```

### 4) FAÇA AS ETAPAS INDICADAS DO SEGUNDO EXEMPLO;
```SQL
INSERT INTO Filmes (titulo, minutos) VALUES ("the terrible trigger" , 120);
INSERT INTO Filmes (titulo, minutos) VALUES ("o alto da compadecida" , 135);
INSERT INTO Filmes (titulo, minutos) VALUES ("faroeste cabloco" , 240);
INSERT INTO Filmes (titulo, minutos) VALUES ("he matrix" , 90);
INSERT INTO Filmes (titulo, minutos) VALUES ("blade runner" , -88);
INSERT INTO Filmes (titulo, minutos) VALUES ("o labirinto do fauno" , 110);
INSERT INTO Filmes (titulo, minutos) VALUES ("metropole" , 0);
INSERT INTO Filmes (titulo, minutos) VALUES ("a lista" , 120);
```
### CHK_MINUTOS
```SQL
DELIMITER $
CREATE TRIGGER chk_minutos BEFORE INSERT ON Filmes 
	FOR EACH ROW 
    BEGIN 
		IF NEW.minutos < 0 THEN
        
				SIGNAL SQLSTATE '4500'
                SET MESSAGE_TEXT = "Valor invalido para minutos" , 
                MYSQL_ERRNO = 2022; 
			
            END IF;
            
		END$
        
DELIMITER ;
```

### LOG_DELETIONS 
```SQL
CREATE TABLE Log_deletions (
id INT PRIMARY KEY AUTO_INCREMENT,
titulo VARCHAR (60),
quando DATETIME,
quem VARCHAR(40)
);

DELIMITER $

CREATE TRIGGER log_deletions AFTER DELETE ON Filmes
	FOR EACH ROW 
    BEGIN 
		INSERT INTO Log_deletions VALUES (NULL, OLD.titulo, sysdate(), user ());
	END$
    
    DELIMITER ;

   DELETE FROM Filmes where id = 2;
   DELETE FROM Filmes where id = 4;
    
    SELECT * FROM log_deletions;
```

### 5) VEJA OS RESULTADOS OBTIDOS A CADA TAREFA REALIZADA E TIRE PRINT’S DOS RESULTADOS;

#### Tabela Pedidos 
![image](https://github.com/GabrielChagasAlves/Trigger/assets/125607847/da4695aa-9df6-467a-ba07-d87ebd5893bf)

#### Tabela Filmes
![image](https://github.com/GabrielChagasAlves/Trigger/assets/125607847/20385c26-463b-4d1b-94ec-389ab9cedc0a)

#### TRIGGER CHK_MINUTOS
![image](https://github.com/GabrielChagasAlves/Trigger/assets/125607847/b0aae12c-e1da-41e1-b4c7-81dae3a5ac83)

#### TRIGGER LOG_DELETIONS
![image](https://github.com/GabrielChagasAlves/Trigger/assets/125607847/6d908503-54e3-4aeb-98bc-7e159d15b2e6)



