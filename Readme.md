import requests
from bs4 import BeautifulSoup
import pandas as pd

# URL da página de resultados
url = "https://www.formula1.com/en/results/2024/races"

# Fazendo uma requisição GET para obter o conteúdo da página
response = requests.get(url)
if response.status_code != 200:
    print("Erro ao acessar o site:", response.status_code)
else:
    # Criando o objeto BeautifulSoup
    soup = BeautifulSoup(response.content, "html.parser")

    # Encontrar a tabela com os resultados (analisar qual tag contém a tabela)
    table = soup.find('table')  # Adapte para o identificador correto da tabela

    # Extração dos dados da tabela
    headers = [header.text.strip() for header in table.find_all('th')]
    rows = table.find_all('tr')

    # Coletando dados da tabela
    data = []
    for row in rows:
        cols = row.find_all('td')
        cols = [ele.text.strip() for ele in cols]
        if cols:
            data.append(cols)

    # Convertendo para DataFrame do pandas
    df = pd.DataFrame(data, columns=headers)

    # Salvando o DataFrame em um arquivo CSV
    df.to_csv('resultados_f1_2024.csv', index=False)

    print("Dados extraídos e salvos com sucesso!")
