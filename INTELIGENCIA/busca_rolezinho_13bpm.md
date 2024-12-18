## Explicação:
Script busca ocorrências que relataram no histórico "Rolezinho", de 2022 em diante, na área do 13 BPM.

```SQL
SELECT oco.numero_ocorrencia,
	oco.data_hora_fato,
	oco.descricao_endereco,
    oco.numero_endereco,
    oco.nome_bairro,
    oco.numero_latitude,
    oco.numero_longitude
FROM db_bisp_reds_reporting.vw_ocorrencia_s oco
WHERE YEAR(oco.data_hora_fato) >= 2022
 	AND oco.nome_municipio = 'BELO HORIZONTE'
 	AND oco.unidade_area_militar_nome LIKE '%13 BPM%'
 	AND (oco.historico_ocorrencia LIKE '%ROLEZINHO%' OR oco.historico_ocorrencia LIKE '%ROLÊZINHO%')
```
