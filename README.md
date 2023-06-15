# UPX-3-FACENS

-- Foi utilizado o SQL Managment Studio 19, da microsoft, para confecção do script de criação e povoamento da base.
-- Um servidor foi criado localmente para testes e consultas.
----------------------------------------------------------

-- Cria tabela clinicas

CREATE TABLE TBL_CLINICAS (
cod_clinica INT PRIMARY KEY,
nome VARCHAR (50) NOT NULL,
cep INT NOT NULL,
numero int NOT NULL,
complemento VARCHAR (10) NOT NULL,
email VARCHAR (50) NOT NULL
)

-- Cria tabela médicos

CREATE TABLE TBL_MEDICOS (
cod_medico INT PRIMARY KEY,
nome VARCHAR (50) NOT NULL,
email VARCHAR (50)
)

-- Cria tabela de cadastro de pacientes

CREATE TABLE TBL_PACIENTES (
    cod_paciente INT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    data_nascimento DATE NOT NULL,
    genero CHAR(1) CHECK (genero IN ('M', 'F')),
    telefone VARCHAR(20),
    email VARCHAR(50),
	cpf VARCHAR(11) NOT NULL UNIQUE,
    senha VARCHAR(20) NOT NULL
);

-- Cria tabela de especialidades médicas
CREATE TABLE TBL_ESPECIALIDADES (
    cod_especialidade INT PRIMARY KEY,
    especialidade VARCHAR(50) NOT NULL
);

-- Cria tabela de associação entre médicos e clinicas 

CREATE TABLE TBL_CADASTRO_MEDICO_ATENDIMENTO (
    cod_clinica INT NOT NULL,
    cod_medico INT NOT NULL,
    PRIMARY KEY (cod_clinica, cod_medico),
    FOREIGN KEY (cod_medico) REFERENCES TBL_MEDICOS(cod_medico),
    FOREIGN KEY (cod_clinica) REFERENCES TBL_CLINICAS(cod_clinica)
);

-- Cria tabela de associação entre médicos e especialidades

CREATE TABLE TBL_CADASTRO_MEDICO_ESPECIALIDADE (
    cod_medico INT NOT NULL,
    cod_especialidade INT NOT NULL,
    PRIMARY KEY (cod_medico, cod_especialidade),
    FOREIGN KEY (cod_medico) REFERENCES TBL_MEDICOS(cod_medico),
    FOREIGN KEY (cod_especialidade) REFERENCES TBL_ESPECIALIDADES(cod_especialidade)
);

-- Cria tabela para agendamento

CREATE TABLE TBL_AGENDA (
    cod_consulta INT PRIMARY KEY,
    cod_medico INT,
    cod_paciente INT,
    cod_clinica INT,
	cod_especialidade INT NOT NULL,
    data_horario_consulta DATETIME,
    FOREIGN KEY (cod_medico) REFERENCES TBL_MEDICOS(cod_medico),
    FOREIGN KEY (cod_paciente) REFERENCES TBL_PACIENTES(cod_paciente),
    FOREIGN KEY (cod_clinica) REFERENCES TBL_CLINICAS(cod_clinica),
	FOREIGN KEY (cod_medico, cod_especialidade) REFERENCES TBL_CADASTRO_MEDICO_ESPECIALIDADE(cod_medico, cod_especialidade),
	FOREIGN KEY (cod_clinica, cod_medico) REFERENCES TBL_CADASTRO_MEDICO_ATENDIMENTO(cod_clinica, cod_medico)
);

-- Povoando o banco de dados
-- Inserir dados na tabela TBL_CLINICAS 


INSERT INTO TBL_CLINICAS (cod_clinica, nome, cep, numero, complemento, email)
VALUES
(1, 'Clinica 1', 11111111, 111, 'Sala 01', 'clinica1@email.com'),
(2, 'Clinica 2', 22222222, 222, 'Sala 02', 'clinica2@email.com'),
(3, 'Clinica 3', 33333333, 333, 'Sala 03', 'clinica3@email.com'),
(4, 'Clinica 4', 44444444, 444, 'Sala 04', 'clinica4@email.com'),
(5, 'Clinica 5', 55555555, 555, 'Sala 05', 'clinica5@email.com'),
(5, 'Clinica 6', 66666666, 666, 'Sala 06', 'clinica6@email.com')

-- Inserir dados na tabela TBL_MEDICOS

INSERT INTO TBL_MEDICOS (cod_medico, nome, email)
VALUES
(1, 'Dr(a). 1', 'd1@email.com'),
(2, 'Dr(a). 2', 'd2@email.com'),
(3, 'Dr(a). 3', 'd3@email.com'),
(4, 'Dr(a). 4', 'd4@email.com'),
(5, 'Dr(a). 5', 'd5@email.com'),
(6, 'Dr(a). 6', 'd6@email.com')

-- Inserir dados na tabela TBL_PACIENTES

INSERT INTO TBL_PACIENTES (cod_paciente, nome, data_nascimento, genero, telefone, email, cpf, senha)
VALUES
(1, 'Paciente 1', '1990-01-01', 'M', '(11) 1111-1111', 'paciente1@email.com', '111.111.111-11', 'senha1'),
(2, 'Paciente 2', '1985-02-15', 'F', '(11) 2222-2222', 'paciente2@email.com', '222.222.222-22', 'senha2'),
(3, 'Paciente 3', '1978-03-28', 'M', '(11) 3333-3333', 'paciente3@email.com', '333.333.333-33', 'senha3'),
(4, 'Paciente 4', '1987-04-02', 'F', '(11) 4444-4444', 'paciente4@email.com', '444.444.444-44', 'senha4'),
(5, 'Paciente 5', '1995-05-11', 'M', '(11) 5555-5555', 'paciente5@email.com', '555.555.555-55', 'senha5'),
(6, 'Paciente 6', '1983-06-23', 'F', '(11) 6666-6666', 'paciente6@email.com', '666.666.666-66', 'senha6')

-- Inserir dados na tabela TBL_ESPECIALIDADES

INSERT INTO TBL_ESPECIALIDADES (cod_especialidade, especialidade)
VALUES
(1, 'Especialidade 1'),
(2, 'Especialidade 2'),
(3, 'Especialidade 3'),
(4, 'Especialidade 4'),
(5, 'Especialidade 5'),
(6, 'Especialidade 6')

-- Inserir dados na tabela TBL_CADASTRO_MEDICO_ATENDIMENTO para especificar qual médico atende em qual clínica

INSERT INTO TBL_CADASTRO_MEDICO_ATENDIMENTO (cod_clinica, cod_medico)
VALUES
(1, 1),
(1, 2),
(2, 2),
(3, 3),
(4, 1),
(4, 4),
(5, 5),
(6, 6)

-- Inserir dados na tabela TBL_CADASTRO_MEDICO_ESPECIALIDADE para especificar qual médico atende qual especialidade

INSERT INTO TBL_CADASTRO_MEDICO_ESPECIALIDADE (cod_medico, cod_especialidade)
VALUES
(1, 1),
(1, 2),
(2, 3),
(3, 3),
(4, 2),
(4, 1),
(5, 6),
(6, 5)

-- Inserir dados na tabela TBL_AGENDA para agendamento de consultas

INSERT INTO TBL_AGENDA (cod_consulta, cod_medico, cod_paciente, cod_clinica, cod_especialidade, data_horario_consulta)
VALUES
(1, 6, 1, 6, 5, '2023-05-15 09:00:00'),
(2, 5, 6, 5, 6, '2023-05-16 10:00:00'),
(3, 4, 2, 1, 1, '2023-05-17 11:00:00'),
(4, 3, 4, 3, 3, '2023-05-15 09:00:00'),
(5, 2, 5, 2, 3, '2023-05-16 10:00:00'),
(6, 1, 3, 2, 1, '2023-05-17 12:00:00')
