## Explicação:
Script com funcionalidade de retornar os veículos que foram roubados na área do 13º BPM, em Belo Horizonte, nos últimos 30 dias EXCETO os veículos que foram recuperados. Portanto o resultado será os veículos que ainda não foram encontrados.

```SQL
SELECT 
    vei.numero_placa,
    vei.marca_modelo_descricao_longa,
    vei.cor_veiculo_descricao_longa,
    vei.data_hora_fato AS data_hora_roubado,
    oco.descricao_endereco AS endereco_roubado,
    oco.numero_endereco AS numero_endereco_roubado,
    oco.nome_bairro AS bairro_roubado,
    oco.numero_latitude AS latitude_roubado,
    oco.numero_longitude AS longitude_roubado
FROM 
    db_bisp_reds_reporting.tb_veiculo_ocorrencia vei
    JOIN db_bisp_reds_reporting.vw_ocorrencia_s oco 
        ON vei.numero_ocorrencia = oco.numero_ocorrencia
WHERE 
    vei.situacao_placa_descricao = 'ROUBADO'
    AND oco.nome_municipio = 'BELO HORIZONTE'
    AND oco.unidade_area_militar_nome LIKE '%13 BPM%'
    AND vei.data_hora_fato >= DATE_ADD(NOW(), INTERVAL -30 DAY) -- Últimos 30 dias
    AND NOT EXISTS (
        SELECT 1
        FROM db_bisp_reds_reporting.tb_veiculo_ocorrencia vei2
        WHERE vei2.numero_placa = vei.numero_placa
        AND vei2.situacao_placa_descricao = 'RECUPERADO'
    );
```
