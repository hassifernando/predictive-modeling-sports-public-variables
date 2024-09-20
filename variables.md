Dados do evento
'Date','Home','Away','Goals_H_HT','Goals_A_HT','Goals_H_FT','Goals_A_FT',
'Odd_H_FT','Odd_D_FT','Odd_A_FT',
'HT_Odd_Over05','HT_Odd_Under05','Odd_Over15_FT','Odd_Under15_FT','Odd_Over25_FT','Odd_Under25_FT','Odd_BTTS_Yes','Odd_BTTS_No',
'handicap': linha handicap do jogo
'handicap_home': odd handicap home
'handicap_away': odd handicap away

TODAS AS INFORMACOES ABAIXO CONSIDERAM APENAS O EVENTO ANTES DELE TER INICIADO, OU SEJA:
- A POSICAO NA TABELA É A POSICAO DOS ENVOLVIDOS ANTES DO EVENTO INICIAR
- A MEDIA DE GOLS MARCADOS, TRATA-SE DA MEDIA ANTES DO EVENTO INICIAR

#power ranking: Segui esse video aqui https://www.youtube.com/watch?v=fVvF1B6sjnw para a construcao desse power ranking
'rank_home','rank_away','rank_home_offense','rank_away_offense','rank_home_defense','rank_away_defense' 

'home_rating': rating_home_att - rating_home_def
'away_rating': rating_away_att - rating_away_def

'rating_home_att','rating_home_def',
'rating_away_att','rating_away_def',
'factor_casa','media_gols_min',
'goals_h_pred','goals_a_pred',

'total_rating' = df['home_rating'] + df['factor_casa'] - df['away_rating']
'sald_gols_jog' = df['goals_h_pred'] - df['goals_a_pred']

#posicao na tabela

'home_rank_home','home_rank_away','home_rank_total',
'away_rank_home','away_rank_away','away_rank_total'

'dif_rank': df['home_rank_total'] - df['away_rank_total']

#probabilidades
'p_h', 'p_d', 'p_a': 1/Odd

Media das probabilidades dos jogos em casa
'p_h_avg_l100', 'p_a_avg_l100',
'p_h_std_l100', 'p_a_std_l100',
'p_h_cv_l100', 'p_a_cv_l100',

Media das probabilidades global
'p_h_avg_glob_l100', 'p_a_avg_glob_l100',
'p_h_std_glob_l100', 'p_a_std_glob_l100',
'p_h_cv_glob_l100', 'p_a_cv_glob_l100'

#INFOS DA LIGA
'num_games_league' : Numero de jogos que ja aconteceram no campeonato
'num_times': numero de times no campeonato
'num_rounds': numero de rodadas que ja ocorreram no campeonato

#GOLS MARCADOS HOME OU AWAY / GLOB
'goals_scored_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'goals_scored_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'goals_scored_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'goals_scored_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'
'goals_scored_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'
'goals_scored_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'

#gols sofridos HOME OU AWAY / GLOB
'goals_conceded_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'goals_conceded_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'goals_conceded_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'goals_conceded_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'
'goals_conceded_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'
'goals_conceded_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'

#media de pontos HOME OU AWAY / GLOBAL
'1_pts_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'1_pts_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'1_pts_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}'
'1_pts_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOBAL'
'1_pts_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOBAL'
'1_pts_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOBAL'

# # # # # # # # # # # # # # # # # #
#CUSTO DO GOL 0
Tive que contornar e colocar com o valor 1 para o custo do gol, onde os os gols sao = 0
![image](https://github.com/user-attachments/assets/116e9262-b839-401c-b8e3-267ed1e69637)
custo_gol_home: p_h/Goals_H_FT
custo_gol_away: p_a/Goals_A_FT

#custo do gol 1
df['custodogol_h_v1'] = df['Goals_H_FT'] / df['p_h']
df['custodogol_a_v1'] = df['Goals_A_FT'] / df['p_a']

#custo do gol 2
df['custodogol_h_v2'] = (df['p_h'] / 2) + (df['Goals_H_FT'] / 2)
df['custodogol_a_v2'] = (df['p_a'] / 2) + (df['Goals_A_FT'] / 2)

# # # # # # # # # # # # # # # # # #
# VALOR DO PONTO
    """
    Descrição: A variável 'Valor do Ponto' ('VP_H' para a equipe da casa e 'VP_A' para a equipe visitante) representa o valor estimado de cada ponto conquistado por uma equipe em um determinado jogo,
    considerando a probabilidade de vitória da equipe e os pontos reais conquistados.
    Essa métrica é calculada multiplicando os pontos conquistados pela equipe (Ptos_H para a equipe da casa e Ptos_A para a equipe visitante) pela
    probabilidade de vitória do oponente (p_A para a equipe da casa e p_H para a equipe visitante).

    Cálculo da média móvel: A média móvel é então calculada para suavizar os valores ao longo do tempo.
    A 'Media_VP_H' representa a média móvel dos valores do ponto para a equipe da casa, enquanto 'Media_VP_A' representa a média móvel dos valores do ponto para a equipe visitante.
    Isso é feito usando a função de média móvel com uma janela de 6 jogos e pelo menos 4 jogos disponíveis para calcular a média.

    Desvio padrão e coeficiente de variação: Além disso, o desvio padrão ('DP_VP_H' para a equipe da casa e 'DP_VP_A' para a equipe visitante) é calculado para medir a dispersão dos valores do ponto em relação à média móvel.
    O coeficiente de variação ('CV_VP_H' para a equipe da casa e 'CV_VP_A' para a equipe visitante) é então calculado dividindo o desvio padrão pela média móvel,
    proporcionando uma medida relativa da dispersão em relação ao valor médio.
    
    Essa variável é útil para avaliar a eficácia de uma equipe em converter oportunidades de vitória em pontos conquistados,
    levando em consideração tanto a probabilidade de vitória quanto o desempenho real da equipe. 
    Valores mais altos podem indicar uma boa eficiência na pontuação,
    enquanto valores mais baixos podem sugerir inconsistências ou dificuldades na conversão de oportunidades em pontos.
    """

df['valor_do_ponto_h'] = df['pts_h'] * df['p_a']
df['valor_do_ponto_a'] = df['pts_a'] * df['p_h']

# # # # # # # # # # # # # # # # # #
# CUSTO DO PONTO
    """
    Conceito:
    O "Custo do Ponto" (CP) é uma métrica que avalia a eficácia das equipes em converter suas chances em pontos, levando em consideração as probabilidades de vitória associadas a essas chances.
    Aqui está uma descrição mais detalhada:

    Interpretação dos Valores:
    Valores Baixos de CP:
    Indicam que a equipe está convertendo suas chances em pontos de forma eficiente, considerando as probabilidades de vitória. Isso sugere uma boa capacidade de aproveitar as oportunidades de pontuação.
    Para equipes com baixo CP, isso significa que estão obtendo bons resultados em relação às suas chances de vencer, o que pode ser atribuído a uma boa eficiência tanto ofensiva quanto defensiva.

    Valores Médios de CP:
    Representam um equilíbrio razoável entre a conversão de chances em pontos e as probabilidades de vitória associadas. As equipes estão conseguindo resultados moderados em relação às suas expectativas.
    Geralmente, valores médios de CP refletem um desempenho padrão ou esperado, sem destacar-se de maneira significativa em termos de eficiência na conversão de chances.

    Valores Altos de CP:
    Indicam que a equipe está tendo dificuldades em converter suas chances em pontos, apesar das probabilidades de vitória favoráveis.
    Isso sugere uma falta de eficiência ofensiva ou defensiva, dependendo do contexto.
    Para equipes com alto CP, isso pode indicar uma subutilização das oportunidades de pontuação ou uma defesa vulnerável, resultando em resultados abaixo do esperado em relação às chances de vencer.

    Importância:
    O CP fornece insights sobre a eficácia das equipes em transformar suas oportunidades em pontos, considerando as probabilidades de vitória.
    Ajuda a identificar áreas de força e fraqueza em termos de eficiência na conversão de chances, permitindo ajustes táticos e estratégicos para melhorar o desempenho da equipe.
    Em resumo, valores baixos de CP indicam eficiência na conversão de chances,
    médios refletem um desempenho padrão e altos destacam áreas de preocupação que precisam ser abordadas para melhorar a eficácia da equipe em pontuar.
    """

df['custo_do_ponto_h'] = df['pts_h'] / df['p_h']
df['custo_do_ponto_a'] = df['pts_a'] / df['p_a']

# # # # # # # # # # # # # # # # # #
# VALOR DO GOL
    """
    Descrição: A variável 'Valor do Gol' ('VG_H' para a equipe da casa e 'VG_A' para a equipe visitante) representa o 
    valor estimado de cada gol marcado por uma equipe em um determinado jogo, considerando a probabilidade de vitória da equipe. 
    Essa métrica é calculada multiplicando os gols marcados pela equipe (Goals_H_FT para a equipe da casa e Goals_A_FT para a equipe visitante) 
    pela probabilidade de vitória do oponente (p_A para a equipe da casa e p_H para a equipe visitante).

    Cálculo da média móvel: Assim como na variável 'Valor do Ponto', a média móvel é calculada para suavizar os valores ao longo do tempo. 
    A 'Media_VG_H' representa a média móvel dos valores do gol para a equipe da casa, enquanto 'Media_VG_A' representa a média móvel dos valores do gol para a equipe visitante. 
    Isso é feito usando a função de média móvel com uma janela de 6 jogos e pelo menos 4 jogos disponíveis para calcular a média.

    Desvio padrão e coeficiente de variação: O desvio padrão ('DP_VG_H' para a equipe da casa e 'DP_VG_A' para a equipe visitante) é calculado para medir a 
    dispersão dos valores do gol em relação à média móvel. O coeficiente de variação ('CV_VG_H' para a equipe da casa e 'CV_VG_A' para a equipe visitante) é então calculado dividindo 
    o desvio padrão pela média móvel, proporcionando uma medida relativa da dispersão em relação ao valor médio.

    Essa variável é útil para avaliar o impacto dos gols marcados por uma equipe em sua performance, considerando tanto a quantidade de gols quanto a probabilidade de vitória. 
    Valores mais altos podem indicar uma boa eficiência na pontuação, enquanto valores mais baixos podem sugerir dificuldades na conversão de oportunidades em gols.
    """

df['valor_do_gol_h'] = df['Goals_H_FT'] * df['p_a']
df['valor_do_gol_a'] = df['Goals_A_FT'] * df['p_h']

# # # # # # # # # # # # # # # # # #
#    SALDO DE GOLS
    ################################################################################

    # Calcula o saldo de gols para casa e fora
df['saldo_gols_h'] = df['Goals_H_FT'] - df['Goals_A_FT']
df['saldo_gols_a'] = df['Goals_A_FT'] - df['Goals_H_FT']

# # # # # # # # # # # # # # # # # #
# Saldo de Gols Ponderado * PROBAB do time
    #################################################################################

    # Calcula o saldo de gols ponderado multiplicado pela probabilidade de vitória do time
df['saldo_gols_ponderado_h_mult_prob_time'] = (df['Goals_H_FT'] - df['Goals_A_FT']) * df['p_h']
df['saldo_gols_ponderado_a_mult_prob_time'] = (df['Goals_A_FT'] - df['Goals_H_FT']) * df['p_a']

# # # # # # # # # # # # # # # # # #
# Saldo de Gols Ponderado / probab do time

df['saldo_gols_ponderado_h_div_prob_time'] = (df['Goals_H_FT'] - df['Goals_A_FT']) / df['p_h']
df['saldo_gols_ponderado_a_div_prob_time'] = (df['Goals_A_FT'] - df['Goals_H_FT']) / df['p_a']

# # # # # # # # # # # # # # # # # #
# Saldo de Gols Ponderado / probabilidade do adversário
df['saldo_gols_ponderado_h_div_prob_adver'] = (df['Goals_H_FT'] - df['Goals_A_FT']) / df['p_a']
df['saldo_gols_ponderado_a_div_prob_adver'] = (df['Goals_A_FT'] - df['Goals_H_FT']) / df['p_h']

# # # # # # # # # # # # # # # # # #
# CUSTO DO SALDO DE GOLS
df['custo_do_saldo_do_gol_h'] = (df['Goals_H_FT'] - df['Goals_A_FT']) / df['p_h']
df['custo_do_saldo_do_gol_a'] = (df['Goals_A_FT'] - df['Goals_H_FT']) / df['p_a']

# # # # # # # # # # # # # # # # # #
# PONTOS ESPERADOS
df['sum_p_mo'] = df['p_h'] + df['p_d'] + df['p_a']
df['juice_mo'] = df['sum_p_mo'] - 1
df['odd_h_real'] = df['Odd_H_FT'] * (df['juice_mo'] + 1)
df['odd_d_real'] = df['Odd_D_FT'] * (df['juice_mo'] + 1)
df['odd_a_real'] = df['Odd_A_FT'] * (df['juice_mo'] + 1)
df['p_h_real'] = df.apply(lambda row: 1 / row['odd_h_real'] if row['odd_h_real'] > 0 else 0, axis=1)
df['p_d_real'] = df.apply(lambda row: 1 / row['odd_d_real'] if row['odd_d_real'] > 0 else 0, axis=1)
df['p_a_real'] = df.apply(lambda row: 1 / row['odd_a_real'] if row['odd_a_real'] > 0 else 0, axis=1)
df['xp_points_real_home'] = (df['p_h_real'] * 3) + (df['p_d_real'] * 1)
df['xp_points_real_away'] = (df['p_a_real'] * 3) + (df['p_d_real'] * 1)


# # # # # # # # # # # # # # # # # #
# DIFERENCA ENTRE PONTOS CONQUISTADOS X PONTOS ESPERADOS
print('Creating variables to check the sum difference between xp points and points - HOME/AWAY ')
df[f'{code}_dif_sum_pts_xp_pts_points_real_h_l{window}'] = df[f'1_pts_sum_h_l{window}'] - df[f'16_xp_points_real_sum_h_l{window}']
df[f'{code}_dif_sum_pts_xp_pts_points_real_a_l{window}'] = df[f'1_pts_sum_a_l{window}'] - df[f'16_xp_points_real_sum_a_l{window}']

print('Creating variables to check the sum difference between xp points and points - global ')
df[f'{code}_dif_sum_pts_xp_pts_points_real_h_glob_l{window}'] = df[f'1_pts_sum_h_l{window}_glob'] - df[f'16_xp_points_real_sum_h_l{window}_glob']
df[f'{code}_dif_sum_pts_xp_pts_points_real_a_glob_l{window}'] = df[f'1_pts_sum_a_l{window}_glob'] - df[f'16_xp_points_real_sum_a_l{window}_glob']

#RATING OFENSIVO / DEFENSIVO
https://www.youtube.com/watch?v=vg5BxFCdYnE&t=764s

#MEDIA DE GOLS PONDERADA PELO TEMPO
https://www.youtube.com/watch?v=6X7R096PXOU&t=588s

# # # # # # # # # # # # # # # # # #
PORCENTAGENS
perc_{home_away}_win: % Vitorias (janela movel) considerando mando de campo
perc_{home_away}_win_global: % Vitorias (janela movel) global

perc_{home_away}_no_loss: % Sem derrotas (janela movel) considerando mando de campo
perc_{home_away}_no_loss_global: % Sem derrotas (janela movel) global

perc_{home_away}_score: % de jogos que o {home_away} marcou um gol
perc_{home_away}_score_15: % de jogos que o {home_away} marcou mais que 1 gol
perc_{home_away}_score_25: % de jogos que o {home_away} marcou mais que 2 gols

perc_{home_away}_conceded: % de jogos que o {home_away} conceded um gol
perc_{home_away}_conceded_15: % de jogos que o {home_away} conceded mais que 1 gol
perc_{home_away}_conceded_25: % de jogos que o {home_away} conceded mais que 2 gols

perc_{home_away}_over_15_goals_total_goals: % de jogos que o {home_away} teve um total de gols superior a 1
perc_{home_away}_over_25_goals_total_goals: % de jogos que o {home_away} teve um total de gols superior a 2

perc_{home_away}_under_15_goals_total_goals: % de jogos que o {home_away} teve um total de gols inferior a 2
perc_{home_away}_under_25_goals_total_goals: % de jogos que o {home_away} teve um total de gols inferior a 3

perc_{home_away}_btts_y: % de jogos que o {home_away} foi ambas marcam sim
perc_{home_away}_btts_n: % de jogos que o {home_away} foi ambas marcam nao

# # # # # # # # # # # # # # # # # #
SEQUENCIAS
streak_{HOME_OU_AWAY}_win: sequencia de vitorias em casa
streak_{HOME_OU_AWAY}_win_global: sequencia de vitorias global

streak_{HOME_OU_AWAY}_no_loss: sequencia de eventos sem derrota em casa
streak_{HOME_OU_AWAY}_no_loss_global: sequencia de eventos sem derrota global

streak_{home_away}_score: sequencia de jogos marcando 1 gol
streak_{home_away}_score_15: sequencia de jogos marcando mais que 1 gol
streak_{home_away}_score_25: sequencia de jogos marcando mais que 2 gols

streak_{home_away}_conceded: sequencia de jogos que o {home_away} conceded um gol
streak_{home_away}_conceded_15: sequencia de jogos que o {home_away} conceded mais que 1 gol
streak_{home_away}_conceded_25: sequencia de jogos que o {home_away} conceded mais que 2 gols

streak_{home_away}_over_15_goals_total_goals: sequencia de jogos que o {home_away} teve um total de gols superior a 1
streak_{home_away}_over_25_goals_total_goals: sequencia de jogos que o {home_away} teve um total de gols superior a 2

streak_{home_away}_under_15_goals_total_goals: sequencia de jogos que o {home_away} teve um total de gols inferior a 2
streak_{home_away}_under_25_goals_total_goals: sequencia de jogos que o {home_away} teve um total de gols inferior a 3

streak_{home_away}_btts_y: sequencia de jogos que o {home_away} foi ambas marcam sim
streak_{home_away}_btts_n: sequencia de jogos que o {home_away} foi ambas marcam nao


