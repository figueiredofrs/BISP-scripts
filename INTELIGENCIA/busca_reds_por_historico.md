## Explicação:
Script que retorna os REDS que atendem às condições abaixo. Nesse caso com natureza C01157 (roubo), na área do 13 BPM e com determinados termos no histórico. Os termos podem ser adicionados ou retirados, além de trocados como necessário.

```SQL
SELECT
	oco.numero_ocorrencia,
	oco.unidade_area_militar_codigo,
	oco.unidade_area_militar_nome,
	oco.nome_bairro,
	oco.nome_municipio,
	oco.natureza_descricao_longa,
	oco.historico_ocorrencia,
	oco.data_hora_fato
FROM
	db_bisp_reds_reporting.vw_ocorrencia_s oco
WHERE
	YEAR (oco.data_hora_fato) >= 2024
	AND MONTH (oco.data_hora_fato) >= 11
	AND oco.natureza_codigo = 'C01157'
	AND oco.unidade_area_militar_nome LIKE '%13 BPM%'
	AND ((oco.historico_ocorrencia LIKE '%MOCHILA IFOOD%')
		OR (oco.historico_ocorrencia LIKE '%IFOOD%')
			OR (oco.historico_ocorrencia LIKE '%BOLSA IFOOD%')
				OR (oco.historico_ocorrencia LIKE '%BOLSAO IFOOD%')
					OR (oco.historico_ocorrencia LIKE '%ENTREGADOR%')
						OR (oco.historico_ocorrencia LIKE '%ENTREGA%'))
```
