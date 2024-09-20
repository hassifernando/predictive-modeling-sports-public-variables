# Dados do evento

'Date',

'Home','Away',

'Goals_H_HT','Goals_A_HT','Goals_H_FT','Goals_A_FT',

'Odd_H_FT','Odd_D_FT','Odd_A_FT',

'HT_Odd_Over05','HT_Odd_Under05',

'Odd_Over15_FT','Odd_Under15_FT',

'Odd_Over25_FT','Odd_Under25_FT',

'Odd_BTTS_Yes','Odd_BTTS_No',

'handicap': linha handicap do jogo

'handicap_home': odd handicap home

'handicap_away': odd handicap away

# NOVOS DADOS PARA FAZER SCRAPPING

![image](https://github.com/user-attachments/assets/4aab64c0-f418-4640-987c-5fe3699d41bd)

    Desempenho, media do time, exemplo da imagem:
        Home: 6.7
        Away: 7.3

![image](https://github.com/user-attachments/assets/aec6fb9d-952d-45dd-bdcb-e7cdf7f59c51)

    Missing Players:
        Numero de missing players de cada time
    Coach:
        Nome do tecnico

<img width="626" alt="image" src="https://github.com/user-attachments/assets/099a39f5-487c-42a1-9ffc-dfc2736c6631">

<img width="669" alt="image" src="https://github.com/user-attachments/assets/93407a44-1061-4ee4-b2ee-1220eec68dfd">

    Statistics

<img width="610" alt="image" src="https://github.com/user-attachments/assets/95307a0c-1d2d-4506-9e62-22f332e9ea85">

    Minuto do gol

    def get_goal_minutes(id_jogo,dictionary,driver):
        def trata_minuto(minuto):
            minuto = minuto
            if "'" in minuto:
                minuto = minuto.replace("'","")
            if ":" in minuto:
                minuto = minuto.split(':')[0]
            if "+" in minuto:
                minuto = minuto.split('+')[0]
            return int(minuto)
        url = f'https://www.flashscore.com/match/{id_jogo}/#/match-summary/match-summary'
        if driver.current_url != url:
            driver.get(url)
        try:
            WebDriverWait(driver, 8).until(EC.visibility_of_element_located((By.CSS_SELECTOR,'div.smv__verticalSections')))
            soup = driver.page_source
            soup = BeautifulSoup(driver.page_source)
            min_goals_home = []
            min_goals_away = []
            try:
                home_incidents = soup.find_all('div',{"class":['smv__homeParticipant']})
                for i in home_incidents:
                    if ('smv__incidentHomeScore' in str(i)) | ('footballOwnGoal-ico' in str(i)):
                        min_goals_home.append(trata_minuto(i.find('div',{"class":['smv__timeBox']}).text))
                dictionary['Goals_Minutes_Home'] = min_goals_home
            except:
                dictionary['Goals_Minutes_Home'] = min_goals_home
            try:
                away_incidents = soup.find_all('div',{"class":['smv__awayParticipant']})
                for i in away_incidents:
                    if ('smv__incidentAwayScore' in str(i)) | ('footballOwnGoal-ico' in str(i)):
                        min_goals_away.append(trata_minuto(i.find('div',{"class":['smv__timeBox']}).text))
                dictionary['Goals_Minutes_Away'] = min_goals_away
            except:
                    dictionary['Goals_Minutes_Away'] = min_goals_away
        except:
            pass   

TODAS AS INFORMACOES ABAIXO CONSIDERAM APENAS O EVENTO ANTES DELE TER INICIADO, OU SEJA:
- A POSICAO NA TABELA É A POSICAO DOS ENVOLVIDOS ANTES DO EVENTO INICIAR
- A MEDIA DE GOLS MARCADOS, TRATA-SE DA MEDIA ANTES DO EVENTO INICIAR


# power ranking: 
Segui esse video aqui https://www.youtube.com/watch?v=fVvF1B6sjnw para a construcao desse power ranking

'rank_home','rank_away','rank_home_offense','rank_away_offense','rank_home_defense','rank_away_defense' 

'home_rating': rating_home_att - rating_home_def

'away_rating': rating_away_att - rating_away_def

'rating_home_att','rating_home_def',

'rating_away_att','rating_away_def',

'factor_casa','media_gols_min',

'goals_h_pred','goals_a_pred',

'total_rating' = df['home_rating'] + df['factor_casa'] - df['away_rating']

'sald_gols_jog' = df['goals_h_pred'] - df['goals_a_pred']


# posicao na tabela

'home_rank_home','home_rank_away','home_rank_total',

'away_rank_home','away_rank_away','away_rank_total'

'dif_rank': df['home_rank_total'] - df['away_rank_total']


# probabilidades
'p_h', 'p_d', 'p_a': 1/Odd


# Media das probabilidades dos jogos em casa
'p_h_avg_l100', 'p_a_avg_l100',

'p_h_std_l100', 'p_a_std_l100',

'p_h_cv_l100', 'p_a_cv_l100',


# Media das probabilidades global
'p_h_avg_glob_l100', 'p_a_avg_glob_l100',

'p_h_std_glob_l100', 'p_a_std_glob_l100',

'p_h_cv_glob_l100', 'p_a_cv_glob_l100'

# INFOS DA LIGA
'num_games_league' : Numero de jogos que ja aconteceram no campeonato

'num_times': numero de times no campeonato

'num_rounds': numero de rodadas que ja ocorreram no campeonato


# GOLS MARCADOS HOME OU AWAY / GLOB
'goals_scored_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'goals_scored_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'goals_scored_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'goals_scored_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'

'goals_scored_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'

'goals_scored_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'


# gols sofridos HOME OU AWAY / GLOB
'goals_conceded_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'goals_conceded_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'goals_conceded_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'goals_conceded_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'

'goals_conceded_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'

'goals_conceded_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOB'


# media de pontos HOME OU AWAY / GLOBAL
'1_pts_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'1_pts_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'1_pts_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}'

'1_pts_avg_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOBAL'

'1_pts_std_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOBAL'

'1_pts_cv_{HOME_OU_AWAY}_l{JANELA_MOVEL}_GLOBAL'

# CUSTO DO GOL 0

Tive que contornar e colocar com o valor 1 para o custo do gol, onde os os gols sao = 0

![image](https://github.com/user-attachments/assets/116e9262-b839-401c-b8e3-267ed1e69637)

    """
    Descrição: A variável 'Custo do Gol 0' ('custo_gol_home' para a equipe da casa e 'custo_gol_away' para a equipe visitante) é uma métrica que mede a relação entre a probabilidade de vitória de uma equipe e os gols marcados. Neste caso, foi implementado um ajuste para evitar divisões por zero, definindo o valor como 1 quando o número de gols é zero.

    Cálculo:
    A fórmula do Custo do Gol 0 avalia quantas unidades de probabilidade de vitória são necessárias para a equipe marcar um gol.
    
    Para a equipe da casa, a fórmula é: Probabilidade de vitória da casa / Gols da casa, representada pela variável custo_gol_home.
    Para a equipe visitante, a fórmula é: Probabilidade de vitória do visitante / Gols do visitante, representada pela variável custo_gol_away.
    Ajuste para gols zero: Quando o número de gols é zero (Goals_H_FT ou Goals_A_FT), o valor do custo do gol é ajustado para 1, evitando problemas de divisão por zero.
    
    Cálculo da média móvel: Assim como nas outras métricas, a média móvel pode ser calculada para suavizar os valores ao longo do tempo. A média de 'custo_gol_home' e 'custo_gol_away' pode ser calculada com uma janela de 6 jogos, com pelo menos 4 jogos disponíveis para o cálculo.
    
    Desvio padrão e coeficiente de variação: O desvio padrão ('DP_custo_gol_home' e 'DP_custo_gol_away') mede a dispersão dos valores do custo do gol em relação à média móvel. O coeficiente de variação ('CV_custo_gol_home' e 'CV_custo_gol_away') é calculado dividindo o desvio padrão pela média móvel.
    
    Interpretação dos valores:
    
    Valores baixos: Indicam que a equipe está marcando gols com uma alta eficiência em relação à sua probabilidade de vitória.
    Valores médios: Refletem uma conversão de gols razoável, com a equipe marcando um número de gols condizente com a probabilidade de vitória.
    Valores altos: Sugerem que a equipe está tendo dificuldade em converter sua probabilidade de vitória em gols, especialmente em casos onde a probabilidade de vitória é alta, mas o número de gols marcados é baixo.
    Importância: O 'Custo do Gol 0' é útil para avaliar a eficiência de uma equipe em transformar sua expectativa de vitória em gols efetivos. Com o ajuste para gols igual a 0, a métrica permite uma avaliação mais consistente, evitando distorções causadas por jogos sem gols. Isso fornece insights valiosos sobre a eficácia ofensiva e pode ajudar na análise de desempenho das equipes.
    """

custo_gol_home: p_h/Goals_H_FT

custo_gol_away: p_a/Goals_A_FT

# CUSTO DO GOL 1
    """
    Descrição: A variável 'Custo do Gol 1' ('custodogol_h_v1' para a equipe da casa e 'custodogol_a_v1' para a equipe visitante) é uma métrica que avalia a eficiência de uma equipe em converter suas chances em gols, levando em consideração as probabilidades de vitória associadas a essas chances.

    Cálculo:
    A fórmula do Custo do Gol 1 compara diretamente os gols marcados pela equipe com a probabilidade de vitória da própria equipe.
    
    Para a equipe da casa, a fórmula é: Gols da casa / Probabilidade de vitória da casa, representada pela variável custodogol_h_v1.
    Para a equipe visitante, a fórmula é: Gols do visitante / Probabilidade de vitória do visitante, representada pela variável custodogol_a_v1.
    Essa abordagem mede o número de gols marcados por cada unidade de probabilidade de vitória da equipe, oferecendo uma visão sobre a capacidade da equipe de transformar a expectativa de vitória em gols efetivos.
    
    Cálculo da média móvel: Para suavizar os valores ao longo do tempo, pode-se calcular a média móvel de 'custodogol_h_v1' e 'custodogol_a_v1'. A média móvel pode ser obtida com uma janela de 6 jogos, exigindo pelo menos 4 jogos disponíveis para o cálculo.
    
    Desvio padrão e coeficiente de variação: O desvio padrão ('DP_custodogol_h_v1' para a equipe da casa e 'DP_custodogol_a_v1' para a equipe visitante) mede a dispersão dos valores do custo do gol em relação à média móvel. O coeficiente de variação ('CV_custodogol_h_v1' e 'CV_custodogol_a_v1') é então calculado dividindo o desvio padrão pela média móvel, fornecendo uma medida relativa da dispersão.
    
    Interpretação dos valores:
    
    Valores baixos: Sugerem que a equipe está sendo eficiente na conversão de suas chances em gols, conseguindo marcar muitos gols em relação à sua probabilidade de vitória.
    Valores médios: Indicam um equilíbrio razoável entre a quantidade de gols marcados e a probabilidade de vitória esperada, sem um desempenho particularmente forte ou fraco.
    Valores altos: Sinalizam dificuldades em converter as chances de vitória em gols, sugerindo que a equipe pode estar desperdiçando oportunidades ou não sendo eficaz em capitalizar sobre sua probabilidade de vitória.
    Importância: O 'Custo do Gol 1' ajuda a medir a eficácia ofensiva das equipes, considerando suas probabilidades de vitória e sua capacidade de converter essas probabilidades em gols. Esta métrica fornece insights sobre como as equipes estão maximizando suas oportunidades, ajudando a identificar áreas que precisam de melhorias, especialmente no aspecto ofensivo.
    """

df['custodogol_h_v1'] = df['Goals_H_FT'] / df['p_h']

df['custodogol_a_v1'] = df['Goals_A_FT'] / df['p_a']

# CUSTO DO GOL 2
    """
    Descrição: A variável 'Custo do Gol 2' ('custodogol_h_v2' para a equipe da casa e 'custodogol_a_v2' para a equipe visitante) é uma métrica alternativa que busca medir a eficiência das equipes em converter suas chances em gols, com base em uma combinação da probabilidade de vitória e o número de gols marcados.

    Cálculo:
    A métrica é calculada pela média ponderada de duas variáveis principais: a probabilidade de vitória da equipe e o número de gols marcados.
    
    Para a equipe da casa, a fórmula é: (Probabilidade de vitória da casa / 2) + (Gols da casa / 2), representada pela variável custodogol_h_v2.
    Para a equipe visitante, a fórmula é: (Probabilidade de vitória do visitante / 2) + (Gols do visitante / 2), representada pela variável custodogol_a_v2.
    Essa abordagem equilibra a probabilidade de vitória e o número de gols marcados, dando peso igual a ambos os fatores na avaliação da eficiência da equipe.
    
    Cálculo da média móvel: Assim como nas outras variáveis, a média móvel pode ser aplicada para suavizar os valores ao longo do tempo. A média móvel de 'custodogol_h_v2' e 'custodogol_a_v2' pode ser calculada usando uma janela de 6 jogos, exigindo pelo menos 4 jogos disponíveis para o cálculo.
    
    Desvio padrão e coeficiente de variação: O desvio padrão ('DP_custodogol_h_v2' para a equipe da casa e 'DP_custodogol_a_v2' para a equipe visitante) mede a dispersão dos valores em relação à média móvel. O coeficiente de variação ('CV_custodogol_h_v2' e 'CV_custodogol_a_v2') é calculado dividindo o desvio padrão pela média móvel, proporcionando uma medida relativa da dispersão.
    
    Interpretação dos valores:
    
    Valores baixos: Indicam que a equipe está convertendo suas chances em gols de forma eficiente, equilibrando bem a probabilidade de vitória e os gols marcados.
    Valores médios: Refletem um desempenho balanceado, com a equipe convertendo suas chances em gols de maneira padrão, sem se destacar nem negativamente nem positivamente.
    Valores altos: Sugerem dificuldades na conversão de chances em gols, mesmo com probabilidades de vitória favoráveis.
    Importância: O 'Custo do Gol 2' oferece uma visão combinada da probabilidade de vitória e da capacidade de converter essa probabilidade em gols. Ele ajuda a identificar a eficácia ofensiva das equipes, ajustando as estratégias para maximizar o aproveitamento de suas oportunidades, tanto em termos de resultado esperado quanto de gols marcados.
    """

df['custodogol_h_v2'] = (df['p_h'] / 2) + (df['Goals_H_FT'] / 2)

df['custodogol_a_v2'] = (df['p_a'] / 2) + (df['Goals_A_FT'] / 2)


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

# SALDO DE GOLS
    """
    Descrição: A variável 'Saldo de Gols' ('saldo_gols_h' para a equipe da casa e 'saldo_gols_a' para a equipe visitante) representa a diferença entre os gols marcados e os gols sofridos por uma equipe em um determinado jogo. Essa métrica é útil para avaliar o desempenho geral de uma equipe em termos de eficiência ofensiva e defensiva.

    Cálculo:
    O Saldo de Gols é calculado subtraindo os gols sofridos pelos gols marcados:
    
    Para a equipe da casa, a fórmula é: Gols da casa - Gols do visitante, representada pela variável saldo_gols_h.
    Para a equipe visitante, a fórmula é: Gols do visitante - Gols da casa, representada pela variável saldo_gols_a.
    Este cálculo mostra a diferença líquida de gols para cada equipe em uma partida, ajudando a avaliar a superioridade ou inferioridade da equipe em termos de placar.
    
    Interpretação dos valores:
    
    Valores positivos: Indicando que a equipe marcou mais gols do que sofreu, um saldo positivo reflete uma performance superior, onde a equipe dominou ofensiva e/ou defensivamente.
    Valores negativos: Significam que a equipe sofreu mais gols do que marcou, indicando uma performance inferior.
    Valores iguais a zero: Mostram que a equipe marcou e sofreu o mesmo número de gols, sugerindo um jogo equilibrado ou empate.
    Importância: O 'Saldo de Gols' é uma métrica chave para avaliar o desempenho de uma equipe ao longo de uma partida ou temporada. Um saldo de gols consistentemente positivo está correlacionado com bons resultados, enquanto um saldo negativo pode indicar problemas na defesa, ataque, ou ambos. Essa métrica também é frequentemente utilizada para desempates em competições e como indicador de força relativa das equipes.
    
    Aplicações adicionais: O saldo de gols também pode ser analisado ao longo do tempo, utilizando médias móveis e outras técnicas de suavização para entender tendências de desempenho da equipe, tanto em jogos em casa quanto fora.
    """
    
df['saldo_gols_h'] = df['Goals_H_FT'] - df['Goals_A_FT']

df['saldo_gols_a'] = df['Goals_A_FT'] - df['Goals_H_FT']

# Saldo de Gols Ponderado * PROBAB do time
    """
    Descrição: A variável 'Saldo de Gols Ponderado Multiplicado pela Probabilidade do Time' ('saldo_gols_ponderado_h_mult_prob_time' para a equipe da casa e 'saldo_gols_ponderado_a_mult_prob_time' para a equipe visitante) combina o saldo de gols de uma equipe com sua probabilidade de vitória, ponderando a diferença de gols pelo quão provável era que a equipe vencesse.
    
    Cálculo:
    Essa métrica é calculada multiplicando o saldo de gols pelo valor da probabilidade de vitória:
    
    Para a equipe da casa, a fórmula é: (Gols da casa - Gols do visitante) * Probabilidade de vitória da casa, representada por saldo_gols_ponderado_h_mult_prob_time.
    Para a equipe visitante, a fórmula é: (Gols do visitante - Gols da casa) * Probabilidade de vitória do visitante, representada por saldo_gols_ponderado_a_mult_prob_time.
    Interpretação dos valores:
    
    Valores positivos: Um valor positivo indica que a equipe marcou mais gols do que sofreu, ponderado por sua probabilidade de vitória. Quanto maior a probabilidade de vitória e o saldo de gols, mais alto será o valor.
    Valores negativos: Significam que a equipe sofreu mais gols do que marcou, e sua probabilidade de vitória não foi capaz de compensar essa diferença.
    Valores próximos de zero: Indicam que a equipe teve um saldo de gols equilibrado, ou que sua probabilidade de vitória era baixa, mesmo em caso de saldo positivo.
    Importância:
    Essa métrica é particularmente útil para entender a performance de uma equipe em relação às expectativas de vitória. Ao ponderar o saldo de gols com a probabilidade de vitória, ela permite uma visão mais ajustada do desempenho. Por exemplo:
    
    Se uma equipe com alta probabilidade de vitória tem um saldo de gols negativo, isso pode indicar um desempenho abaixo do esperado.
    Por outro lado, uma equipe com baixa probabilidade de vitória, mas com saldo de gols positivo, pode estar superando expectativas.
    Aplicações adicionais:
    Assim como outras métricas, essa variável pode ser usada para análise de tendências ao longo de várias partidas, utilizando médias móveis ou outras técnicas de suavização. A ponderação pelo valor da probabilidade também pode ajudar a ajustar modelos preditivos e comparações entre equipes com diferentes níveis de expectativa.
    """
df['saldo_gols_ponderado_h_mult_prob_time'] = (df['Goals_H_FT'] - df['Goals_A_FT']) * df['p_h']

df['saldo_gols_ponderado_a_mult_prob_time'] = (df['Goals_A_FT'] - df['Goals_H_FT']) * df['p_a']

# Saldo de Gols Ponderado / probab do time
    """
    Descrição: A variável 'Saldo de Gols Ponderado Dividido pela Probabilidade do Time' ('saldo_gols_ponderado_h_div_prob_time' para a equipe da casa e 'saldo_gols_ponderado_a_div_prob_time' para a equipe visitante) avalia o saldo de gols de uma equipe ajustado pela sua probabilidade de vitória, dividindo a diferença de gols marcados e sofridos pela probabilidade de vitória da equipe.
    
    Cálculo:
    O saldo de gols é ponderado pela probabilidade de vitória, sendo ajustado para refletir a expectativa de performance:
    
    Para a equipe da casa, a fórmula é: (Gols da casa - Gols do visitante) / Probabilidade de vitória da casa, representada por saldo_gols_ponderado_h_div_prob_time.
    Para a equipe visitante, a fórmula é: (Gols do visitante - Gols da casa) / Probabilidade de vitória do visitante, representada por saldo_gols_ponderado_a_div_prob_time.
    Interpretação dos valores:
    
    Valores elevados: Um valor elevado indica que a equipe teve um saldo de gols positivo (ou seja, marcou mais gols do que sofreu) e uma probabilidade de vitória relativamente baixa, o que sugere um desempenho acima do esperado.
    Valores baixos ou negativos: Um valor baixo ou negativo indica que o saldo de gols da equipe foi negativo, ou que a equipe teve um saldo positivo, mas com uma probabilidade de vitória alta, o que pode indicar que a performance foi dentro do esperado ou abaixo dele.
    Valores próximos de zero: Sinalizam que a equipe teve um saldo de gols equilibrado em relação à sua probabilidade de vitória.
    Importância:
    Essa métrica oferece uma visão ajustada do desempenho de uma equipe em relação à expectativa de vitória, considerando não apenas o resultado em termos de gols, mas também o quanto esse desempenho estava alinhado (ou desalinhado) com a probabilidade de vitória. Ela é útil para identificar:
    
    Superações de expectativas: Quando uma equipe com baixa probabilidade de vitória consegue um bom saldo de gols.
    Desempenhos abaixo do esperado: Quando uma equipe com alta probabilidade de vitória apresenta um saldo de gols negativo ou baixo.
    Aplicações adicionais:
    Esta variável pode ser usada em modelos preditivos para ajustar o desempenho relativo das equipes, e também pode ser suavizada ao longo de várias partidas por meio de médias móveis para observar tendências de performance, tanto em casa quanto fora.
    """

df['saldo_gols_ponderado_h_div_prob_time'] = (df['Goals_H_FT'] - df['Goals_A_FT']) / df['p_h']

df['saldo_gols_ponderado_a_div_prob_time'] = (df['Goals_A_FT'] - df['Goals_H_FT']) / df['p_a']

# Saldo de Gols Ponderado / probabilidade do adversário
    """
    Descrição: A variável 'Saldo de Gols Ponderado Dividido pela Probabilidade do Adversário' ('saldo_gols_ponderado_h_div_prob_adver' para a equipe da casa e 'saldo_gols_ponderado_a_div_prob_adver' para a equipe visitante) pondera o saldo de gols de uma equipe ao dividir a diferença de gols marcados e sofridos pela probabilidade de vitória do time adversário. Isso ajusta o saldo de gols pelo nível de desafio imposto pela probabilidade do oponente.
    
    Cálculo:
    O saldo de gols é ajustado pela probabilidade de vitória do adversário:
    
    Para a equipe da casa, a fórmula é: (Gols da casa - Gols do visitante) / Probabilidade de vitória do visitante, representada por saldo_gols_ponderado_h_div_prob_adver.
    Para a equipe visitante, a fórmula é: (Gols do visitante - Gols da casa) / Probabilidade de vitória da casa, representada por saldo_gols_ponderado_a_div_prob_adver.
    Interpretação dos valores:
    
    Valores elevados: Um valor elevado indica que a equipe teve um saldo de gols positivo, mesmo enfrentando um adversário com alta probabilidade de vitória. Isso sugere que a equipe teve um bom desempenho contra um adversário teoricamente mais forte.
    Valores baixos ou negativos: Um valor baixo ou negativo indica que a equipe teve dificuldades, apresentando um saldo de gols negativo, ou que seu desempenho foi abaixo do esperado frente a um adversário considerado mais fraco.
    Valores próximos de zero: Apontam para uma partida equilibrada, em que o saldo de gols ajustado pelo nível de desafio do adversário resultou em um desempenho compatível com as probabilidades.
    Importância:
    Essa métrica permite avaliar o desempenho de uma equipe em relação à qualidade do adversário, conforme indicado pela probabilidade de vitória do oponente. É útil para:
    
    Identificar performances excepcionais: Quando uma equipe supera as expectativas contra um adversário difícil.
    Avaliar desempenhos abaixo do esperado: Quando uma equipe com vantagem teórica enfrenta dificuldades contra um adversário com menor probabilidade de vitória.
    Aplicações adicionais:
    O uso dessa variável em análise de tendências ou modelos preditivos pode ajudar a ajustar o desempenho de equipes, levando em consideração a força do adversário. Também é possível utilizar médias móveis ou outras formas de suavização para analisar o comportamento ao longo de várias partidas.
    """
df['saldo_gols_ponderado_h_div_prob_adver'] = (df['Goals_H_FT'] - df['Goals_A_FT']) / df['p_a']

df['saldo_gols_ponderado_a_div_prob_adver'] = (df['Goals_A_FT'] - df['Goals_H_FT']) / df['p_h']

# CUSTO DO SALDO DE GOLS
    """
    Descrição: A variável 'Custo do Saldo de Gols' ('custo_do_saldo_do_gol_h' para a equipe da casa e 'custo_do_saldo_do_gol_a' para a equipe visitante) quantifica o saldo de gols de uma equipe em relação à sua probabilidade de vitória. Essa métrica representa o "custo" de cada gol marcado ou sofrido, ajustado pela probabilidade de vitória da equipe.

    Cálculo:
    O saldo de gols é ajustado pela probabilidade de vitória da equipe, da seguinte forma:
    
    Para a equipe da casa, a fórmula é: (Gols da casa - Gols do visitante) / Probabilidade de vitória da casa, representada por custo_do_saldo_do_gol_h.
    Para a equipe visitante, a fórmula é: (Gols do visitante - Gols da casa) / Probabilidade de vitória do visitante, representada por custo_do_saldo_do_gol_a.
    Interpretação dos valores:
    
    Valores elevados: Um valor elevado indica que a equipe obteve um saldo de gols positivo e ajustado para uma probabilidade de vitória relativamente baixa, sugerindo um desempenho eficiente, ou um bom saldo de gols mesmo com chances menores de vitória.
    Valores baixos ou negativos: Um valor baixo ou negativo indica um saldo de gols negativo, ou que o saldo de gols positivo foi alcançado em um contexto de alta probabilidade de vitória, sugerindo que a equipe teve dificuldades ou performou dentro do esperado.
    Valores próximos de zero: Indicativos de partidas equilibradas, onde o saldo de gols e a probabilidade de vitória estão em alinhamento, sem grandes discrepâncias.
    Importância:
    Essa métrica é útil para avaliar a eficiência das equipes ao converter sua probabilidade de vitória em saldo de gols efetivo. Pode ser aplicada para:
    
    Identificar a eficiência ofensiva/defensiva: Se uma equipe está convertendo bem suas chances de vitória em saldo de gols positivo.
    Avaliar performances abaixo do esperado: Se o saldo de gols está abaixo do esperado, considerando a alta probabilidade de vitória.
    Aplicações adicionais:
    O 'Custo do Saldo de Gols' pode ser usado para ajustar modelos preditivos de desempenho, refletindo a capacidade de uma equipe em maximizar seu saldo de gols de acordo com suas probabilidades. Isso permite insights sobre a consistência ou vulnerabilidade de uma equipe em situações com diferentes probabilidades de vitória.
    """
df['custo_do_saldo_do_gol_h'] = (df['Goals_H_FT'] - df['Goals_A_FT']) / df['p_h']

df['custo_do_saldo_do_gol_a'] = (df['Goals_A_FT'] - df['Goals_H_FT']) / df['p_a']

# PONTOS ESPERADOS

    """
    Descrição: A variável 'Pontos Esperados' ('xp_points_real_home' para a equipe da casa e 'xp_points_real_away' para a equipe visitante) representa o número de pontos que uma equipe pode esperar conquistar com base nas probabilidades reais ajustadas pelas odds do mercado, considerando as probabilidades de vitória, empate e derrota.
    
    Cálculo:
    
    Sumário das probabilidades de mercado:
    Inicialmente, as probabilidades de vitória da casa (p_h), empate (p_d), e vitória da visitante (p_a) são somadas para calcular sum_p_mo. A partir disso, o 'juice' de mercado (juice_mo) é determinado subtraindo 1 de sum_p_mo.
    
    Ajuste das odds reais:
    As odds de vitória da casa (Odd_H_FT), empate (Odd_D_FT), e vitória da visitante (Odd_A_FT) são ajustadas pelo valor do 'juice', gerando odds reais (odd_h_real, odd_d_real, odd_a_real).
    
    Probabilidades reais:
    As probabilidades reais de vitória da casa (p_h_real), empate (p_d_real), e vitória da visitante (p_a_real) são então calculadas usando o inverso das odds reais, levando em consideração um valor positivo de odds.
    
    Cálculo dos pontos esperados:
    
    Para a equipe da casa: xp_points_real_home é obtido multiplicando a probabilidade real de vitória da casa (p_h_real) por 3 pontos, somado à probabilidade real de empate (p_d_real) multiplicada por 1 ponto.
    Para a equipe visitante: xp_points_real_away segue a mesma lógica, utilizando a probabilidade real de vitória da visitante (p_a_real) e de empate (p_d_real).
    Interpretação dos valores:
    
    Pontos esperados altos: Indicativos de um cenário onde a equipe tem uma alta probabilidade de obter um bom resultado (vitória ou empate), baseado nas odds ajustadas do mercado.
    Pontos esperados baixos: Refletem uma expectativa de desempenho inferior ou uma baixa probabilidade de conquistar pontos no jogo, de acordo com as odds de mercado ajustadas.
    Importância:
    Os Pontos Esperados fornecem uma estimativa mais precisa sobre o desempenho projetado de uma equipe com base nas probabilidades reais derivadas das odds ajustadas. Essa métrica é útil para:
    
    Avaliar a expectativa de desempenho: Comparando as odds de mercado com o desempenho real, é possível identificar se a equipe está superando ou ficando abaixo das expectativas.
    Ajustar modelos preditivos: Incorporando essa métrica, modelos de previsão podem ser refinados para melhorar a precisão das estimativas de resultados futuros.
    Aplicações adicionais:
    Esta métrica pode ser usada em análises avançadas de eficiência de mercado, ajudando a determinar o quão bem as odds refletem a realidade do desempenho das equipes e permitindo a identificação de discrepâncias que possam ser exploradas em modelos de apostas ou estratégias preditivas.
    """

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


# DIFERENCA ENTRE PONTOS CONQUISTADOS X PONTOS ESPERADOS
    """
    Descrição: A variável 'Diferença entre Pontos Conquistados e Pontos Esperados' calcula a discrepância entre os pontos efetivamente conquistados por uma equipe e os pontos que eram esperados com base nas probabilidades ajustadas do jogo. Essa métrica é calculada separadamente para a equipe da casa e para a equipe visitante, permitindo uma análise mais detalhada do desempenho relativo.

    Cálculo:
    
    Diferença Local:
    
    Para a equipe da casa: A diferença é dada pela fórmula Pontos Conquistados da Casa - Pontos Esperados da Casa, representada por dif_sum_pts_xp_pts_points_real_h_l{window}.
    Para a equipe visitante: A fórmula é Pontos Conquistados da Visitante - Pontos Esperados da Visitante, representada por dif_sum_pts_xp_pts_points_real_a_l{window}.
    Diferença Global:
    
    Para a equipe da casa: dif_sum_pts_xp_pts_points_real_h_glob_l{window} calcula a diferença entre os pontos conquistados ao longo do tempo e os pontos esperados globalmente para a casa.
    Para a equipe visitante: dif_sum_pts_xp_pts_points_real_a_glob_l{window} calcula a diferença para a equipe visitante sob as mesmas condições.
    Interpretação dos valores:
    
    Valores positivos: Indicam que a equipe superou as expectativas, conquistando mais pontos do que o esperado com base nas odds ajustadas. Isso sugere um desempenho acima do esperado.
    Valores negativos: Indicam que a equipe não conseguiu atingir suas expectativas, conquistando menos pontos do que o previsto. Isso pode sugerir dificuldades no desempenho ou um desvio significativo entre as expectativas de desempenho e os resultados reais.
    Valores próximos de zero: Apontam para uma correspondência próxima entre os pontos conquistados e os pontos esperados, sugerindo que a equipe está performando de acordo com as expectativas.
    Importância:
    Essa métrica é essencial para avaliar a eficácia de uma equipe em converter suas oportunidades em resultados reais, permitindo identificar áreas de desempenho forte ou fraco. Pode ser utilizada para:
    
    Análise de performance: Ajuda a avaliar se as equipes estão maximizando suas chances de conquistar pontos com base nas probabilidades do mercado.
    Ajuste de estratégias: Identificar equipes que consistentemente superam ou ficam aquém das expectativas pode auxiliar na formulação de estratégias táticas e de treinamento.
    Aplicações adicionais:
    A diferença entre pontos conquistados e pontos esperados pode ser integrada em análises mais amplas de desempenho, permitindo insights sobre a eficácia do time e ajustes em modelos preditivos para resultados futuros, influenciando decisões em apostas e gestão de equipes.
    """
df[f'{code}_dif_sum_pts_xp_pts_points_real_h_l{window}'] = df[f'1_pts_sum_h_l{window}'] - df[f'16_xp_points_real_sum_h_l{window}']

df[f'{code}_dif_sum_pts_xp_pts_points_real_a_l{window}'] = df[f'1_pts_sum_a_l{window}'] - df[f'16_xp_points_real_sum_a_l{window}']

df[f'{code}_dif_sum_pts_xp_pts_points_real_h_glob_l{window}'] = df[f'1_pts_sum_h_l{window}_glob'] - df[f'16_xp_points_real_sum_h_l{window}_glob']

df[f'{code}_dif_sum_pts_xp_pts_points_real_a_glob_l{window}'] = df[f'1_pts_sum_a_l{window}_glob'] - df[f'16_xp_points_real_sum_a_l{window}_glob']

# RATING OFENSIVO / DEFENSIVO
https://www.youtube.com/watch?v=vg5BxFCdYnE&t=764s

# MEDIA DE GOLS PONDERADA PELO TEMPO
https://www.youtube.com/watch?v=6X7R096PXOU&t=588s

# PORCENTAGENS

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

# SEQUENCIAS

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

# PROXIMAS VARIAVEIS QUE EU VOU TENTAR CRIAR

    failed to score
    clean sheet
    Home advantage: https://www.soccerstats.com/table.asp?league=brazil&tid=ha
    Relative form: https://www.soccerstats.com/table.asp?league=brazil&tid=re
    Goals ranges: https://www.soccerstats.com/table.asp?league=brazil&tid=9#google_vignette
    Relative perf: https://www.soccerstats.com/table.asp?league=brazil&tid=rp
    Goal margins: https://www.soccerstats.com/table.asp?league=brazil&tid=e
    
    Criar variaveis que se relacionam, exemplo:
        Odd Home vs Media Odd_Home



