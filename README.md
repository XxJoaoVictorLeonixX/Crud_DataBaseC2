# Trabalho c2

Repositorio para o trabalho da c2 de banco de Dados Ministrada Pelo Professor Howard Cruz 

## Tema - Campeonato de Futebol :soccer:

Este projeto é um sistema de gerenciamento de partidas de futebol desenvolvido em Python e SQL, criado como parte de um trabalho acadêmico da matéria de Banco de dados do professor Howard Cruz. Ele oferece uma solução  para controlar jogadores, times e resultados de partidas. Com este sistema, você pode cadastrar,cadastrar, visualizar e analisar informações sobre times, jogadores e partidas 

Com os dados fornecidos rodada a rodada todos os dados são convertidos em uma tabela com a classificação do campeonato e outros dados, gerando assim uma classificação na junção de todas as tabelas e é revelado quem foi o campeão e a classificação geral.


## Como Rodar a aplicação :hammer:

Esse sistema de Gerenciar campeonato de futebo é composto por um conjunto de tabelas que representam um campeonato no modelo de pontos corridos, contendo tabelas como: Jogadores, Times, Partidas.

O sistema exige que as tabelas existam, então basta executar o script Python a seguir para criação das tabelas e preenchimento de dados de exemplos:
```shell
~$ python create_tables_and_records.py
```

Para executar o sistema basta executar o script Python a seguir:
```shell
~$ python principal.py
```

Para que possa testar as conexões com o banco de dados Oracle e o módulo Conexion desenvolvido para esse projeto, basta executar o script Python a seguir:
```shell
~$ python test.py
```

## Organização 📁 
- [diagrams](diagrams): Nesse diretório está o [diagrama relacional](diagrams/DIAGRAMA_RELACIONAL_PEDIDOS.pdf) (lógico) do sistema.
    * O sistema possui cinco entidades: PRODUTOS, CLIENTES, FORNECEDORES, PEDIDOS e ITENS_PEDIDO
- [sql](sql): Nesse diretório estão os scripts para criação das tabelas e inserção de dados fictícios para testes do sistema
    * Certifique-se de que o usuário do banco possui todos os privilégios antes de executar os scripts de criação, caso ocorra erro, execute o comando a seguir com o superusuário via SQL Developer: `GRANT ALL PRIVILEGES TO LABDATABASE;`
    * [create_tables_pedidos.sql](sql/create_tables_pedidos.sql): script responsável pela criação das tabelas, relacionamentos e criação de permissão no esquema LabDatabase.
    * [inserting_samples_records.sql](sql/inserting_samples_records.sql): script responsável pela inserção dos registros fictícios para testes do sistema.
    * [inserting_samples_related_records.sql](sql/inserting_samples_related_records.sql): script responsável pela inserção dos registros fictícios de pedidos e itens de pedidos para testes do sistema utilizando blocos PL/SQL.
- [src](src): Nesse diretório estão os scripts do sistema
    * [conexion](src/conexion): Nesse repositório encontra-se o [módulo de conexão com o banco de dados Oracle](src/conexion/oracle_queries.py). Esse módulo possui algumas funcionalidades úteis para execução de instruções DML e DDL, sendo possível obter JSON, Matriz e Pandas DataFrame.
      - Exemplo de utilização para consultas simples:

        ```python
        def listar_Times(self, oracle:OracleQueries, need_connect:bool=False):
        query = """
                select t.times
                , t.id_time
                , t.nome
                from times t
                order by t.id_time
                """
        if need_connect:
            oracle.connect()
        print(oracle.sqlToDataFrame(query))

        ```
      - Exemplo de utilização para alteração de registros
     
      - def excluir_times(self):
        oracle = OracleQueries(can_write=True)
        oracle.connect()

        id_time = int(input("Id do time que deseja Excluir: "))        

        if not self.verifica_existencia_time(oracle, id_time):            
            df_time = oracle.sqlToDataFrame(f"select id_time, nome from times where id_time = {id_time}")
            oracle.write(f"delete from times where id_time = {id_time}")            
            time_excluido = Times(df_time.id_time.values[0], df_time.nome.values[0])
            print("Time Removido com Sucesso!")
            print(time_excluido.to_string())
        else:
            print(f"O Id:{id_time} do time não existe.")

     
    def inserir_times(self) -> Times:
        ''' Ref.: https://cx-oracle.readthedocs.io/en/latest/user_guide/plsql_execution.html#anonymous-pl-sql-blocks'''
        
     
        -id_time = input("Id Time (novo): ")

        if self.verifica_existencia_times(oracle, id_time):
            nome_time = input("Nome do time (Novo): ")
            oracle.write(f"insert into times values ('{id_time}', '{nome_time}'")
            df_times = oracle.sqlToDataFrame(f"select id_time, nome from times where id_time = '{id_time}'")
            novoTime = Times(df_times.id_time.values[0], df_times.nome.values[0])
            print(novoTime.to_string())
            return novoTime
        else:
            print(f"O time {id_time} já está cadastrado.")
            return None
  

        ```python
        from conexion.oracle_queries import OracleQueries
        def inserir_Times(self) -> Time:
            # Cria uma nova conexão com o banco que permite alteração
            oracle = OracleQueries(can_write=True)
            oracle.connect()

           
         
      - Outros exemplos: [test.py](src/test.py)
      - Caso esteja utilizando na máquina virtual antiga, você precisará alterar o método connect de:
          ```python
          self.conn = cx_Oracle.connect(user=self.user,
                                  password=self.passwd,
                                  dsn=self.connectionString()
                                  )
          ```
        Para:
          ```python
          self.conn = cx_Oracle.connect(user=self.user,
                                  password=self.passwd,
                                  dsn=self.connectionString(in_container=True)
                                  )
          ```
    * [controller](src/controller/): Nesse diretório encontram-sem as classes controladoras, responsáveis por realizar inserção, alteração e exclusão dos registros das tabelas.
    * [model](src/model/): Nesse diretório encontram-ser as classes das entidades descritas no [diagrama relacional](diagrams/DIAGRAMA_RELACIONAL_PEDIDOS.pdf)
    * [reports](src/reports/) Nesse diretório encontra-se a [classe](src/reports/relatorios.py) responsável por gerar todos os relatórios do sistema
    * [sql](src/sql/): Nesse diretório encontram-se os scripts utilizados para geração dos relatórios a partir da [classe relatorios](src/reports/relatorios.py)
    * [utils](src/utils/): Nesse diretório encontram-se scripts de [configuração](src/utils/config.py) e automatização da [tela de informações iniciais](src/utils/splash_screen.py)
    * [create_tables_and_records.py](src/create_tables_and_records.py): Script responsável por criar as tabelas e registros fictícios. Esse script deve ser executado antes do script [principal.py](src/principal.py) para gerar as tabelas, caso não execute os scripts diretamente no SQL Developer ou em alguma outra IDE de acesso ao Banco de Dados.
    * [principal.py](src/principal.py): Script responsável por ser a interface entre o usuário e os módulos de acesso ao Banco de Dados. Deve ser executado após a criação das tabelas.

### Bibliotecas Utilizadas
- [requirements.txt](src/requirements.txt): `pip install -r requirements.txt`

#### Em caso de problemas com a execução dos software dando a seguinte mensagem `ORA-28001: the password has expired`, execute as linhas de comando a seguir no Oracle:
- `ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;`
- `ALTER USER labdatabase IDENTIFIED BY "labDatabase2022";`
- `ALTER USER labdatabase IDENTIFIED BY  "labDatabase2022";`

### Instalando Oracle InstantClient
- Baixe a versão do [InstantClient](https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html) de acordo com a versão do Banco de Dados
- Caso esteja utilizando uma distribuição Linux baseado em Debian, será necessário executar o comando a seguir para converter o arquivo .rpm para .deb.
  ```shell
  sudo alien --scripts oracle-instantclient18.5-basic-18.5.0.0.0-3.x86_64.rpm
  ```
- Descompacte o arquivo e será gerado um diretório em um diretório de fácil acesso.
- Mova os diretórios lib e share para dentro do diretório do InstantClient
  ```shell
  sudo mv lib /usr/local/oracle/instantclient_18_5/
  ```
  
  ```shell
  sudo mv share instantclient_18_5/
  ```
- Edite o arquivo `.bash_profile` incluindo as linhas a seguir ao final do arquivo:
  ```shell
  export ORACLE_HOME=/usr/local/oracle/instantclient_18_5/lib/oracle/18.5/client64
  export LD_LIBRARY_PATH=$ORACLE_HOME/lib
  export PATH=$PATH:$ORACLE_HOME/bin
  export PATH
  ```


## Integrantes
*João Victor Leoni dos santos
*Lucas Fraga de Andrade
*Daniel José Holz 
*Gabriel dos Santos
*Jhean Virginio Perim Pazetto
*Guilherme Barbosa Medici Loureiro 
*Maria Eduarda André Carlete 

