# Análise de textura em imagens digitais: GLCM e medidas de Haralick

Tutorial de teoria e prática (Projeto 2, opção 2) da disciplina de Computação Visual, Universidade Presbiteriana Mackenzie, Faculdade de Computação e Informática, Ciência da Computação. Professor: André Kishimoto.

## Integrantes

| Nome completo | RA |
|---|---|
| Arthur Silva Santana | 10420550 |
| Richard Barbosa Sanches | 10420179 |
| Totti Ferreira Gomes | 10428490 |

## O que é este projeto

Um notebook Jupyter que explica e testa uma pergunta simples: dá para descrever uma textura (grama, madeira, tecido, tijolo) com números, e esses números bastam para distinguir uma textura da outra?

O notebook tem uma parte teórica e uma parte prática em Python:

- **Teoria:** o que é textura, estatísticas de primeira ordem (média, desvio padrão e entropia do histograma), estatísticas de segunda ordem com a matriz de coocorrência de níveis de cinza (GLCM) e as cinco medidas de Haralick usadas (contraste, dissimilaridade, homogeneidade, energia e correlação).
- **Prática:** cálculo das medidas em recortes de imagens Brodatz, implementação manual da GLCM conferida contra o scikit-image, efeito da distância e do ângulo, e classificação com k-vizinhos mais próximos (k-NN) para verificar se as medidas separam as texturas.

## Escopo

Textura estatística em imagens em escala de cinza. Ficam de fora: textura colorida, mapeamento de textura em modelos 3D, LBP, filtros de Gabor, redes neurais e segmentação por textura.

## Conteúdo do notebook

1. Teoria: o que é textura?
2. Estatísticas de primeira ordem
3. Estatísticas de segunda ordem: a GLCM
4. Medidas de Haralick
5. Prática: dados (patches de textura)
6. Prática: primeira ordem
7. Prática: GLCM manual vs. scikit-image
8. Prática: medidas por classe
9. Prática: efeito da distância e do ângulo
10. Prática: classificação com k-vizinhos mais próximos
11. Conclusões
12. Referências

## Parâmetros do experimento

| Parâmetro | Valor |
|---|---|
| Níveis de cinza (quantização) | 32 |
| Distâncias | 1, 3 e 5 pixels |
| Ângulos | 0°, 45°, 90° e 135° |
| Medidas de Haralick | 5 (60 valores por patch, 5 x 3 x 4) |
| Tamanho do patch | 64 x 64 pixels, sem sobreposição |
| Imagens | grama, madeira, tecido e tijolo (Brodatz), 56 patches por classe, 224 no total |
| Classificador | k-NN com k = 5, características padronizadas |

## Principais resultados

Acurácia do k-NN em dois tipos de avaliação. Na validação cruzada (A) os patches de teste são sorteados no meio dos de treino. Na separação espacial (B) o treino usa uma região da foto e o teste usa outra, com uma linha de patches descartada entre as duas.

| Conjunto de características | A: validação cruzada | B: separação espacial |
|---|---|---|
| 1ª ordem (3 características) | 0,929 | 0,823 |
| GLCM, média dos ângulos (15) | 0,982 | 0,917 |
| GLCM completa (60) | 1,000 | 0,979 |

- A primeira ordem separa bem a grama, mas não enxerga a organização espacial: embaralhar os pixels de um patch de tecido não muda média, desvio nem entropia.
- Nenhuma medida sozinha separa as quatro classes. Madeira e tijolo têm contraste quase igual (6,673 e 6,303) e só se separam combinando medidas e mantendo os ângulos separados.
- A distância controla a escala: o contraste cresce e a correlação cai de d = 1 para d = 5 em todas as classes.
- O ângulo mostra a direção da textura: madeira e tijolo têm contraste muito menor a 90° (0,61 e 3,01 em d = 1).
- A validação cruzada dá resultados mais altos que a separação espacial. Para comparar os conjuntos, a separação espacial é a avaliação mais confiável.

**Limitações:** cada classe é uma única fotografia, então a separação espacial testa outras regiões da mesma foto e não outras fotos de grama, madeira, tecido ou tijolo. Também foram usadas só três distâncias.

## Como executar

Testado no Windows com Python 3.13.17, NumPy 2.5.4, Matplotlib 3.11.2, scikit-image 0.26.0 e scikit-learn 1.9.1.

1. Instale o Python 3.13 e as bibliotecas:

   ```
   py -3.13 -m pip install numpy matplotlib scikit-image scikit-learn jupyter
   ```

   (Em outros sistemas, use `python -m pip install ...`.)

2. Clone ou baixe este repositório e mantenha a estrutura de pastas abaixo. O notebook procura as imagens em `brodatz/`, ao lado do arquivo `.ipynb`.

3. Abra `tutorial_textura_glcm.ipynb` no VS Code (com a extensão Jupyter) ou rode `jupyter notebook` na pasta do projeto, e execute todas as células de cima para baixo (Run All). A semente aleatória é fixa (42), então os números impressos se repetem a cada execução.

## Estrutura do repositório

```
tutorial_textura_glcm.ipynb
README.md
brodatz/
    grass.tiff
    wood_grain.tiff
    Herringbone_weave.tiff
    brick_wall.tiff
```

## Imagens

As quatro imagens vêm do banco USC-SIPI, Volume 1: Textures, que reúne as texturas do álbum de Phil Brodatz. Elas estão incluídas aqui apenas para que o notebook possa ser executado. Segundo a nota do próprio banco, as imagens Brodatz de 512 x 512 têm zeros nas duas últimas linhas, e o notebook remove essas linhas antes de usá-las.

## Vídeo de apresentação

https://youtu.be/z7NmD6g0hSw

## Referências

1. HARALICK, R. M.; SHANMUGAM, K.; DINSTEIN, I. Textural features for image classification. *IEEE Transactions on Systems, Man, and Cybernetics*, v. SMC-3, n. 6, p. 610-621, 1973.
2. BRODATZ, P. *Textures: A Photographic Album for Artists and Designers.* New York: Dover, 1966.
3. GONZALEZ, R. C.; WOODS, R. E. *Digital Image Processing.* 4. ed. Pearson, 2018.
4. VAN DER WALT, S. et al. scikit-image: image processing in Python. *PeerJ*, v. 2, e453, 2014.
5. HARRIS, C. R. et al. Array programming with NumPy. *Nature*, v. 585, p. 357-362, 2020.
6. PEDREGOSA, F. et al. Scikit-learn: machine learning in Python. *Journal of Machine Learning Research*, v. 12, p. 2825-2830, 2011.
7. USC-SIPI Image Database, Volume 1: Textures. University of Southern California. Disponível em: https://sipi.usc.edu/database/. Acesso em: 26 out. 2026.
8. Documentação do scikit-image: *GLCM Texture Features* (`skimage.feature.graycomatrix` e `graycoprops`).
