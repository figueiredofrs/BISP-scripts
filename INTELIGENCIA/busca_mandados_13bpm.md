## Explicação:
O script retorna os indivíduos que figuraram na condição de autor, co-autor ou suspeito, em REDS de 2022 em diante, onde a último REDS desse indivíduo foi registrado como endereço do mesmo os bairros em questão. 

```SQL
SELECT sip.numero_sip, 
	sip.numero_registro_geral,
	sip.nome_do_individuo,
	sip.nome_da_mae,
	sip.data_nascimento_formatada,
	sip.mandadosabertos,
	CAST (oco.data_hora_fato AS date) AS datafato,
	env.envolvimento_descricao,
	env.natureza_ocorrencia_descricao_longa,
	oco.unidade_area_militar_nome,
	env.nome_municipio,
	env.nome_bairro,
	env.descricao_endereco,
	env.numero_endereco
FROM db_bisp_sip_reporting.vw_individuo_sip sip
	JOIN db_bisp_reds_reporting.tb_envolvido_ocorrencia env
 		on sip.numero_sip = env.numero_individuo_sip
 	JOIN db_bisp_reds_reporting.tb_ocorrencia oco
 		on oco.numero_ocorrencia = env.numero_ocorrencia 
 WHERE env.envolvimento_descricao IN ('AUTOR','CO-AUTOR','SUSPEITO')
 	AND YEAR(oco.data_hora_fato) >= 2022
 	AND env.nome_municipio = 'BELO HORIZONTE'
 	AND sip.mandadosabertos <> 0
 	AND env.nome_bairro IN ('BACURAU','CAMPO ALEGRE','ITAPOA','JARDIM ATLANTICO','PLANALTO','VILA CLORIS','BIQUINHAS','HELIOPOLIS','SAO BERNARDO','SAO TOMAZ','VILA AEROPORTO','AEROPORTO','INDAIA','JARAGUA','LIBERDADE','SANTA ROSA','SAO LUIZ','VILA AEROPORTO JARAGUA','VILA RICA','VILA SANTO ANTONIO','DONA CLARA','SAO FRANCISCO','SUZANA','UNIVERSITARIO','VILA REAL II','VILA SANTA ROSA','VILA SUZANA I','VILA SUZANA II', 'AARAO REIS','BOA UNIAO I','BOA UNIAO II','CONJUNTO PROVIDENCIA','MINASLANDIA','PRIMEIRO DE MAIO','PROVIDENCIA','SAO GONCALO','VILA MINASLANDIA','VILA PRIMEIRO DE MAIO','CONJUNTO FLORAMAR','FLORAMAR','GUARANI','JARDIM FELICIDADE','JARDIM GUANABARA','SOLIMOES','GRANJA WERNECK','LAJEDO','MARIA TERESA','MIRANTE','MONTE AZUL','NOVO AARAO REIS','NOVO TUPI','RIBEIRO DE ABREU','TUPI A','TUPI B','CANAA','ETELVINA CARNEIRO','FREI LEOPOLDO','JAQUELINE','JULIANA','MADRI','MARIQUINHAS','SAO DAMIAO','SATELITE','VILA NOVA','XODO-MARIZE','ZILAH SPOSITO')
```
