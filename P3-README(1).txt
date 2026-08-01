# ===========================================================
# Identificacao do grupo:  [T?? para Tagus ou A?? para Alameda]
# T27
# Membros [istID, primeiro + ultimo nome]
# 1.César Dianov
# 2.Eduardo Rocha
# 3.Ana Rodrigues
$
# ===========================================================
# Descricao da ISA Implementada
#
# == Formato das Instrucoes ==
# Indicar a divisao dos campos da instrucao
# -cada intruçao possui o opcode que sao os ultimos 2 bits do numero binario que vem da ROM:
#     00->li 
#     01->addi
#     10->subi
#     11->relu/abs
# -tirando relu e o abs,os restantes bits das restantes intruçoes representam o imediato(6 bits)
# -para distinguir o relu do abs verificamos o 3º bit a contar da direita,
#     011->relu
#     111->abs
# -como nestas 2 intruçoes nao presisamos de imediatos,os restantes 5 digitos podem ser valores aleatorios
#
#
# Justificar decisoes: Por que escolheram esse numero de bits? Ha instrucoes com formatos diferentes?
# -escolhemos apenas 2 bits para representar as intruçoes porque com 2 bits podemos representar 4 intruçoes diferentes
# -para depois diferenciar o relu do abs usamos mais um bit(isto é valido pois nao precisamos de imediatos)

# == Sumario dos Estagios do Pipeline==
# Descrever brevemente cada estagio (componentes de hardware utilizados)
  O PC vai iterando sobre as intruçoes na ROM(pc possui a msm estrutura que nas aulas de lab),
  cada intruçao é separada(splitter) em duas partes,0->1 o opcode ,e de 2->7 o valor de imediato,
  o opcode vai servir como seletor no Mux principal e cada entrada vai ser o resultado de uma operaçao
  Para a soma foi usado um adder,com a entrada A sendo o valor do regsito e a entrada B o imediato
  A subtraçao possui a mesma estrutura que a soma ,só que ao invés de se usar um adder,usou-se um subtrator
  Para o load immed apenas ligamos o imediato diretamente a entrada 0 do mux
  O relu saca o valor atual no registo e verifica o sinal(- ou +),isolando o primeiro digito do imediato(splitter),
     esse valor(0 ou 1) vai servir de selector no Mux relu.O resultado do Mux relu irá pro Mux relu/abs
  O Abs saca tambem o valor no resgito ,verifica se é negativo ou positivo.Se for negativo ele nega o valor todo(Gate Not)
     e soma 1(passar valor negativo a positivo com complmento para 2).O resultado do Mux abs irá pro Mux relu/abs 
  Finalmente em Mux abs/relu ,é selecionado o valor que passa adiante atraves do 3º digito da intruçao vinda da ROM.
     esse valor é mandado para MUX principal



# == Sinais de Controlo ==
# Explicar o que cada sinal ativa/desativa/seleciona e como sao gerados.
# -neste processador usamos 4 MUX(cada um devidamente identificado),havendo 4 selectores,que sao:
# -selector ligado ao "MUX principal" -> opcode das intruçoes -> seleciona a entrada que pode passar adiante,correspondete ao opcode da intruçao pretendida
#     ,ou seja se o opcode for 10,o valor que está na entrda 2 passa(no caso o resultado da subtraçao)
# -selector do "MUX relu" -> primeiro digito do valor guardado no regsito(verifica se é positivo/negativo) ->se for negativo(1),o valor é passado a 0
#                                                                                                          ->se for positivo(0),o valor mantem-se
# -selector do "MUX abs" -> primeiro digito do valor guardado no regsito(verifica se é positivo/negativo) ->se for negativo(1),o valor é passado a positivo
#                                                                                                           ->se for positvo(0),o valor mantem-se
# ->selector do "MUX relu/abs" ->3º digito do valor vindo da ROM -> pode ser 0 ou 1 ->se 0,o resultado de relu passa adiante('0'11)
#                                                                                   ->se 1,o resultado de abs passa adiante ('1'11)
# ===========================================================
# Requisitos do enunciado que *nao* estao corretamente implementados:
# (indicar um por linha, ou responder "nenhum")
#
# -o nosso processador 8-bit Spike apenas suporta valores detro do intervalo -32 a 31,
#  nao podendo executar a intruçao "li 33" 

# ===========================================================
# Top-3 das otimizacoes que a vossa solucao incorpora:
# (maximo 140 caracteres por cada otimizacao)
#
# 1.
#
# 2.
#
# 3.
#
# ===========================================================
