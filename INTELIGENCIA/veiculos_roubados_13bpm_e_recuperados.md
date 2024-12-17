## Explicação:
Esse script fornece como resultado os veículos que foram roubados no 13º BPM, Belo Horizonte, mas que foram recuperados em qualquer parte do estado de MG, desde que os dados estejam disponíveis na BISP. O filtro 'BELO HORIZONTE' e '13 BPM' aplica-se apenas aos que estão constando como roubados.

```SQL
SELECT 
    roubado.numero_placa,
    roubado.marca_modelo_descricao_longa,
    roubado.cor_veiculo_descricao_longa,
    -- Informações de quando foi ROUBADO
    roubado.data_hora_fato AS data_hora_roubado,
    oco_roubado.descricao_endereco AS endereco_roubado,
    oco_roubado.numero_endereco AS numero_endereco_roubado,
    oco_roubado.nome_bairro AS bairro_roubado,
    oco_roubado.numero_latitude AS latitude_roubado,
    oco_roubado.numero_longitude AS longitude_roubado,
    -- Informações de quando foi RECUPERADO
    recuperado.data_hora_fato AS data_hora_recuperado,
    oco_recuperado.descricao_endereco AS endereco_recuperado,
    oco_recuperado.numero_endereco AS numero_endereco_recuperado,
    oco_recuperado.nome_bairro AS bairro_recuperado,
    oco_recuperado.numero_latitude AS latitude_recuperado,
    oco_recuperado.numero_longitude AS longitude_recuperado
FROM 
    db_bisp_reds_reporting.tb_veiculo_ocorrencia roubado
    -- Join com a tabela para recuperar a ocorrência ROUBADA
    JOIN db_bisp_reds_reporting.vw_ocorrencia_s oco_roubado 
        ON roubado.numero_ocorrencia = oco_roubado.numero_ocorrencia
    -- Self join para encontrar o mesmo veículo na situação RECUPERADO
    JOIN db_bisp_reds_reporting.tb_veiculo_ocorrencia recuperado
        ON roubado.numero_placa = recuperado.numero_placa
    -- Join com a tabela para recuperar a ocorrência RECUPERADA
    JOIN db_bisp_reds_reporting.vw_ocorrencia_s oco_recuperado
        ON recuperado.numero_ocorrencia = oco_recuperado.numero_ocorrencia
WHERE 
    roubado.situacao_placa_descricao = 'ROUBADO'
    AND recuperado.situacao_placa_descricao = 'RECUPERADO'
    AND oco_roubado.nome_municipio = 'BELO HORIZONTE'
    AND oco_roubado.unidade_area_militar_nome LIKE '%13 BPM%'
    AND roubado.data_hora_fato >= DATE_ADD(NOW(), INTERVAL -30 DAY) -- Últimos 30 dias
```
