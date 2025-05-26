# projeto2_analisedados
Projeto desenvolvido no Bootcamp de Análise de Dados da Laboratória


Ficha Técnica
Projeto de Análise de Dados - Hipóteses Gravadora

Duração: 2-3 sprints (semanas)
Equipe: Dupla (Anielle)

✅ Descrição do objetivo:

Durante o projeto, fui anotando que o objetivo principal é validar hipóteses sobre o que influencia o sucesso de músicas nas plataformas de streaming (Spotify, Deezer, Apple Music). Queremos entender quais características, como BPM, presença em playlists, número de músicas do artista, impactam no número de streams para ajudar a gravadora e o artista a tomar decisões estratégicas.

🔧 Pré-processamento de dados:

Anotei que os dados foram limpos principalmente via views no BigQuery, tratando valores nulos sem apagar (apenas substituí) e investigando duplicados com duas abordagens: apagar duplicados criando uma view limpa e criar outra view que sinaliza duplicados sem removê-los. Ajustei dados inconsistentes, como valores "00:00" no campo track_id, substituindo por "0000000" em todas as tabelas para padronizar. Fiz consultas para identificar problemas e usei views para organizar o processo. Também registre que números em campos de texto foram analisados para decidir o tratamento.

📊 Métodos e técnicas:

Durante a análise exploratória, realizei cálculos de quartis, correlações e criei tabelas e gráficos para entender padrões. Apliquei técnicas de segmentação para validar as hipóteses levantadas. Também conectei a base de dados limpa ao Power BI para criar um dashboard visual que ajuda a interpretar os dados e apresentar resultados.

✅ Conclusões:

A partir dos dados, pude confirmar ou refutar algumas hipóteses da gravadora, como a correlação entre BPM e número de streams, impacto da presença em playlists e a influência do número de músicas do artista. As descobertas forneceram recomendações estratégicas para a gravadora, mostrando quais fatores realmente influenciam o sucesso nas plataformas de streaming.

⚠️ Limitações:

Anotei que os dados tinham limitações como valores nulos e duplicados que precisaram ser tratados com cuidado para não comprometer a análise. Algumas variáveis textuais apresentaram desafios no pré-processamento. Além disso, a análise está limitada ao conjunto de dados disponível, que representa apenas 2023 e pode não capturar todas as nuances do mercado musical.

💬 Comentários e anotações:

Preferi usar views encadeadas para organizar o pré-processamento, mas fiquei em dúvida se deveria ter criado uma view única final para juntar tudo.

Criei documentações das consultas, o que ajuda para projetos futuros, especialmente para lidar com valores nulos e duplicados.

Mantive registros de todas as decisões para facilitar o entendimento e reprodutibilidade.

Fiz uso constante dos links e vídeos recomendados para entender melhor o BigQuery e as técnicas de análise.

🛠 Ferramentas e Tecnologias:

Google BigQuery (SQL, views)

Power BI (dashboard)

Apresentações Google (para apresentação do projeto)

Loom (gravação da apresentação em vídeo)

