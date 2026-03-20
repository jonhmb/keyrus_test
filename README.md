README

##### Bem vinda ao meu teste. Procurei seguir exatamente o mesmo padrão de todos os projetos que já trabalhei como Engenheiro de Dados. Espero que goste!

## Arquitetura
Irei fazer requisição dos dados do IBGE através da API via Python/PySpark e persistir os dados 'crus' na camada bruta do datalake em .json, depois irei processar esses dados (limpando, tornando confiáveis) e persistir nas DeltaTables camada silver, e por fim irei formar as views finais para consumo do PowerBi na camada gold e consumir esses dados com o PowerBI
<img width="4162" height="952" alt="arquitetura" src="https://github.com/user-attachments/assets/dbc83e8d-d0ba-4086-be69-ab979015754e" />


## Ordem cronológica da execução (encontra-se em Data_Engineer_Test/ETL): 
1. bronze/bronze_01_ingest_ibge_populacao
2. bronze/bronze_02_ingest_ibge_pib
3. silver/silver_01_clean_populacao
4. silver/silver_02_clean_pib
5. gold/gold_01_indicadores_municipio

## Modelagem
Separei a tabela de população das dimensões de tempo e localidade facilitando consultas e deixando a estrutura mais organizada. E se necessário evoluir o projeto com novos dados futuramente, é possível adicionar novas tabelas sem precisar alterar o que já existe e garanti a rastreabilidade dos dados.
### Camada Bronze ->
ingest_ibge_populacao:
	-id
	-json: json do request
	-createdAt: data e hora da ingestão

ingest_ibge_pib:
	-id
	-json: json do request
	-createdAt: data e hora da ingestão

### Camada silver -> 
clean_populacao_estado:
	-id
	-estado: UF
	-createdAt: data e hora da criação
	-deletedAt: data e hora da deleção
	-raw_id: id de rastreio da camada raw (bronze)

clean_populacao_cidade:
	-id
	-cidade: município
	-createdAt: data e hora da criação
	-deletedAt: data e hora da deleção
	-id_estado: referencia a tabela estado

clean_populacao_habitantes:
	-id
	-qtd_populacao: número de habitantes
	-createdAt: data e hora da criação
	-deletedAt: data e hora da deleção
	-id_cidade: referencia a tabela cidade

clean_pib_pib:
	-id
	-pib_2021: número de habitantes
	-createdAt: data e hora da criação
	-deletedAt: data e hora da deleção
	-id_cidade: referencia a tabela cidade

### Camada gold -> 
indicadores_municipio:
	-nome cidade
	-uf
	-ano
	-pib
	-POPULACAO_QTD
	-pib_per_capita
	-share_uf
	-share_brasil
	-ranking_uf
	-ranking_brasil

indicadores_uf:
	-uf
	-ano
	-populacao_total
	-pib_total
	-pib_per_capita_medio

indicadores Brasil:
	-ano
	-pib_total_brasil	
	-populacao_total_brasil
	-pib_per_capita_medio_brasil
	-share_uf_max
	-share_brasil_max
	-ranking_uf_top
	-ranking_brasil_top


# Relatório no PowerBI:
[relatorio powerbi.pdf](https://github.com/user-attachments/files/26140202/relatorio.powerbi.pdf)
