# Python

**Fazer download de bases de MIS**

import time
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
import sys
import zipfile
import os

from importlib import reload 
reload(sys)
sys.path.append("C:\\ProgramData\\Anaconda3\\Lib\\site-packages\\chromedriver_binary-89.0.4389.23.0-py3.8.egg")

import chromedriver_binary

#Abrindo navegador e baixando arquivos
navegador = webdriver.Chrome()
navegador.get("http://gestao-doc.internal.timbrasil.com.br/sites/MISCSMO/Churn%20Detalhado/202110_CHURN_DETALHADO.zip")
navegador.get("http://gestao-doc.internal.timbrasil.com.br/sites/MISCSMO/Migracao%20Detalhada/202110_MIGRACAO_TOTAL.zip")



#Aguardando 5 minutos pelo download dos arquivos
time.sleep(300)


#Alternando para o diretório do projeto
os.chdir("C:\\Users\\f8079641\\OneDrive - TIM\\1. Matheus\\1. MIS")


#Extraindo arquivos
with zipfile.ZipFile('C:\\Users\\f8079641\\Downloads\\202110_CHURN_DETALHADO.zip', 'r') as zip_ref:
             zip_ref.extractall()

with zipfile.ZipFile('C:\\Users\\f8079641\\Downloads\\202110_MIGRACAO_TOTAL.zip', 'r') as zip_ref:
             zip_ref.extractall()         
             
os.remove('C:\\Users\\f8079641\\Downloads\\202110_CHURN_DETALHADO.zip')
os.remove('C:\\Users\\f8079641\\Downloads\\202110_MIGRACAO_TOTAL.zip')


navegador.quit()

**Fazer download de bases de MIS BAT**

salvar como RPA_Portal_MIS.bat

CD "C:\Users\f8079641\OneDrive - TIM\1. Matheus\07 Python"
PYTHON RPA_Portal_MIS.py

------------------------------------------------------------------------------

**Appendar arquivos do bilhete**

#!/usr/bin/env python
# coding: utf-8

# In[1]:

# Biblioteca para manipulação de excel e etc.
import pandas as pd
# Biblioteca para manipulação do sistema operacional.
import os
# Biblioteca para seleção de arquivos padronizados.
import glob
# Biblioteca para manipulação de arquivos (copy and paste).
import shutil

# In[2]:

# Caminho onde estão os arquivos a serem appendados.
source = ('C:\\Users\\f8079641\\OneDrive - TIM\\1. Matheus\\1. Pos_SMB\\SMB_MDG\\2. Relatorios_Atualizaçoes_de_Bases\\10_Portabilidade\\00_Recebido')


# In[3]:


# Cria o caminho para cada CSV contido na pasta de destino
all_files_source = glob.glob(source + "/*.txt")


# In[4]:


# Uma verificação para ver se criou um caminho para cada csv na pasta.
#print(all_files_source)


# In[5]:


# Pega todo o conteúdo da pasta destino e gera uma lista appendada.
lista01 = []
for txt in all_files_source:
    df = pd.read_csv(txt, engine = 'python', encoding = 'ISO-8859-1', sep = '|', quotechar='"', error_bad_lines=False) # o csv possui um erro, uma linha de nome de cliente com uma aspas, o interpretador não consegue ler, então estamos pulando esses casos
    df['FILENAME'] = os.path.basename(txt)
    lista01.append(df)


# In[6]:


# confere o append.
# df.head()


# In[7]:

# Concatena o append em um única lista.
lista02 = pd.concat(lista01)

# In[8]:

# Um print para validar.
# lista02.head()

# In[9]:

# Caminho para onde sera salvo o arquivo appendado.
destination = ('C:\\Users\\f8079641\\OneDrive - TIM\\1. Matheus\\1. Pos_SMB\\SMB_MDG\\2. Relatorios_Atualizaçoes_de_Bases\\10_Portabilidade\\01_BASE')

# In[10]:


# Cria o arquivo appendado no caminho já determinado no base
lista02.to_csv(destination + "\\bs_po.txt")

