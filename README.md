# Diagnóstico de Índices Redundantes e Sem Uso (SQL Server)

Este projeto contém um script transacional avançado para identificar **índices sem leitura (Zero Reads)** ou com **chaves redundantes** no SQL Server. O objetivo principal é auxiliar DBAs e desenvolvedores na recuperação de espaço em disco e no alívio de gargalos de I/O em ambientes de produção.

---

## Alertas Vitais de Produção

Antes de executar qualquer comando de exclusão (`DROP`), avalie cuidadosamente os seguintes pontos:

1. **Uptime não é garantia:** O script analisa estatísticas baseadas no ciclo do `tempdb`. Mesmo que o servidor esteja online há mais de 30 dias, rotinas trimestrais, semestrais ou fechamentos anuais podem ficar de fora da amostragem.
2. **Recriação básica vs. Rollback fiel:** O comando DDL de rollback gerado de forma automática é simplificado. Ele **não mapeia** propriedades avançadas como `FILLFACTOR`, `DATA_COMPRESSION`, índices filtrados (`WHERE`) ou ordenação específica de colunas (`DESC`). **Sempre faça o backup do DDL original antes de prosseguir.**
3. **Atenção ao Resumo Executivo:** No Passo 3 do script, se múltiplos índices cobrirem o mesmo candidato, a operação de `JOIN` pode inflar os números de espaço estimado. Encare este resumo como uma métrica macro.
4. **Ponto de Partida:** O script entrega índices *candidatos à revisão*, e não uma ordem de exclusão direta. Use-o como base para a sua auditoria técnica.

---

## Como Utilizar

1. Abra o arquivo do script no SQL Server Management Studio (SSMS) ou Azure Data Studio.
2. Altere a linha inicial `USE db_MEU_BANCO;` para o nome do banco de dados que deseja analisar.
3. Execute o script.
4. Analise os resultados divididos em três etapas:
   * **Passo 1:** Semáforo de confiabilidade com base no uptime do servidor.
   * **Passo 2:** Relatório detalhado com o diagnóstico do índice, comando pronto de `DROP` e comando automático de `ROLLBACK`.
   * **Passo 3:** Resumo macro do ganho estimado de espaço e escritas desperdiçadas que serão evitadas.

---

## Licença

Este projeto está sob a licença MIT. Sinta-se livre para usar, modificar e compartilhar.
