
# Modelagem de um ECG com o framework CHESS

Este documento contém de forma detalhada o processo de modelagem e análise dos resultados de confiabilidade e disponibilidade de um ECG para um ambiente de UTI cardíaca. 

Este documento está organizado primeiramente na modelagem do ECG, contendo as visões a nível de sistema e também a nível de *software*, e após isso como foi aplicada as análises de confiabilidade e disponibilidade nos modelos já desenvolvidos do ECG. Também neste mesmo documento, existe a estrutura de como a modelagem esta organizada arquiteturalmente dentro do CHESS.


# System View
a descrever...

# Software View
A visão de *software* é responsável por proporciar que o modelador realize abstrações do que se pretende modelar a nível de *software*. Para o ECG foram realizadas 5 abstrações para modelagem, que são:

 1. Detecção de doenças;
 2. Bateria;
 3. Fluxo do sinal capturado dentro do ECG;
 4. Tela de resultados que o ECG coletou de um ou mais pacientes.
 5. Monitor multiparametrico

Durante a modelagem foram definidos alguns padrões para facilitar a legibilidade dos diagramas. Os *ComponentTypes* são os blocos de cor vermelha, os *ComponentImplementations* são os blocos de cor verde, já as interfaces possuem a cor amarela, e os *Component* que possuem um diagrama de composição representando o fluxo de todo o diagrama de classe possui a cor cinza.

Cada *ComponentType* deve possuir um *ComponentImplementation* associado a ele. Cada *ComponentType* podem possuir relação com interfaces, sendo essas interfaces categorizadas em prover (saída) ou requerer (entrada). Se um *ComponentType* requer uma interface, é criado um relacionamento de realização e esse relacionamento, já se um *ComponentType* prover uma interface, é criado um relacionamento de dependência o *ComponentType* e a interface. Os relacionamentos criados a partir das portas de comunicação entre os ComponentTypes e as Interfaces é possível observar nos diagramas de classes. Para criar o relacionamento entre um *ComponentType* e uma interface, é necessário criar em cada *ComponentType* um diagrama de composição, e então neste diagrama de composição, criar portas do esteriótipo *ClientServerPort*, onde na porta defini-se o nome da porta, o tipo que é a interface, e se ela é uma interface *required*(entrada) ou *provided*(saída). Ao definir as portas em cada ComponentType atribuindo as interfaces, os relacionamentos de realização ou dependência são criados.

Em cada diagrama de classe existe um componente do tipo *Component* na cor cinza que representa toda a comunicação definida entre os *Components* e os tipos de interface no diagrama de classes. Como em todos os componetes do tipo *ComponentType* possuem um diagrama de composição onde são definidos as portas de comunicação utilizando as interfaces. Como já mencionado no paragrafo anterior, ao definir essas portas, são criadas os relacionamentos de dependencia e realização no diagrama de classes. Outro ponto importante para ser detalhado, no diagrama de classes, para cada *ComponentType* é necessário definir um *ComponentImplementation*. Ao definir um *ComponentImplementation* deve-se criar manualmente um relacionamento de realização do *ComponentImplementation* para o *ComponentType*. Ao criar o relacionamento de realização entre um *ComponentType* e um *ComponentImplementation*, ele herda definições do *ComponentType*, como métodos, operações, portas de comunicação e etc.

Após definir o diagrama de classe com todos os componentes e interfaces necessários, deve-se criar um diagrama de composição utilizando um componente do tipo *Component* que irá representar toda a comunicação dos componentes e interfaces definidos no diagrama de classes. Neste diagrama de composição que representa o diagrama de classes são utilizados os *ComponentImplementations* que herdam as informações dos *ComponentTypes* que estão associados. Os *ComponentImplementations* possuem as portas herdadas do *ComponentType* associado, e assim elas podem ser utilizadas no diagrama de composição para realizar a comunicação entre os componentes.

## Estrutura da modelagem

Esta modelagem necessita possuir uma organização arquitetural, assim facilita a manutenabilidade e o entedimento de terceiros ao ver a modelagem e consigar localizar o que for necessário. A nível de software, a modelagem do ECG foi organizada prioritariamente e separada em packages representando as principais modelagens. Dentro de cada package, irão ser encontradas os components, interfaces, diagramas e etc sobre cada modelagem.

![enter image description here](https://i.imgur.com/R0Fn5zy.png)

Dentro de cada modelagem, todos possuem o mesmo padrão. Ao explorar uma modelagem específica como por exemplo a modelagem de AlarmBattery, será possível perceber que existem pacotes que são responsáveis por armazenar as Interfaces, ComponentTypes, ComponentImplementations, Conectores e Diagramas, e dentro de diagramas separados em diagramas de compsição e diagramas de classes, como mostra a figura a seguir.

![enter image description here](https://i.imgur.com/GzSOiBn.png)

## Detecção de doenças

No diagrama de classes para a modelagem de um detector de doenças que trabalha em conjunto com o ECG, foi necessário definir dois ComponentsTypes, um responsável por receber o sinal, e um outro por realizar a detecção de doença. O primeiro ComponentType definido chamado de ReceiveSignal em seu diagrama de composição possui uma porta de comunicação, onde o seu direcionamento é prover a interface ReceiveSignal. Essa definição é possível observar na imagem a seguir.

![enter image description here](https://i.imgur.com/HUoS8YU.png)

Após definir o diagrama de composição para o *ComponentType* *ReceiveSignal*, é necessário definir o diagrama de composição para o *ComponentType* *DetectDiseases*. O *ComponentType* *DetectDiseases* possui 7 portas de comunicação com as interfaces definidas no diagrama de classes. A primeira porta é uma porta de entrada, onde o ComponentType DetectDiseases requer uma interface para iniciar o seu funcionamento. Esta interface requerida é a interface ReceiveSignal, onde o *ComponentType* *ReceiveSignal* prover a mesma. Após o *ComponentType* DetectDiseases receber a interface ReceiveSignal, o componente está pronto para poder encaminhar para uma de suas portas de saída a doença detectada. O diagrama de composição para o ComponentType DetectDiseases, pode ser observado na imagem abaixo.

![enter image description here](https://i.imgur.com/xROa2iD.png)

Cada interface que o ComponentType DetectDiseases possui um relacionamento de realização, representa uma doença detectada, que são: ataque cardíaco, arritimia, insuficiência cardíaca, parada cardíaca, taquicardia e bradicardia. Essas anomalias cardíacas, são anomalias que um ECG pode detectar em um ambiente de UTI.

Com a definição das Interfaces, ComponentTypes e ComponentsImplementations, o diagrama de classes para detecão de doenças com os componentes e seus relacionamentos entre eles pode ser observado na imagem a seguir.

![enter image description here](https://i.imgur.com/lAkVGiZ.png)

Após definir o diagrama de classes para a detecção de doenças, é necessário criar um diagrama de composição. É necessário definir no diagrama de classes um componente do tipo *Component* que representa o fluxo e interações dos componentes definidos no diagrama de classes. Esse component definido foi nomeado de *DetectDiseasesSW*, e é possível observar ele na cor cinza no diagrama de classes. 

O *ComponentImplementation* *ReceiveSignal_impl* prover uma porta de saída nomeada de *send_signal_to_detect* do tipo *ReceiveSignal*. Esta porta realiza uma comunicação com a porta *r_signal* do *ComponentImplementation* *DetectDiseases_impl* que possui o mesmo tipo de interface *ReceiveSignal*. O *ComponentImplementation* *DetectDiseases_impl* ao receber em uma porta de entrada o *ReceiveSignal*, tem a função de realizar a detecção de anomalias cardíacas, e de acordo com a anomalia cardíaca detectada, cada uma pode ir para a sua porta de saída específica. Cada porta de saída do *DetectDiseases_impl* representa uma anomalia cardíaca detectada. O diagrama de composição para o Component DetectDiseases pode ser observado na figura a seguir.

![Diagrama de composição para detecção de doenças](https://i.imgur.com/x0WIzxg.png)

## Bateria

Para a utilização do ECG e de outros componentes, inicialmente é preciso veerificar se a bateria possui uma carga suficiente para que o ECG inicie a captura do sinal, se a bateria estiver com uma carga abaixo do aceitável, o ECG não deve iniciar o procedimento de captura de sinal e deve emitir um alarme informando que a bateria possui um baixo nível de carga. Se a bateria possuir uma carga aceitável para iniciar o procedimento, o ECG deve informar a carga da bateria atual e então iniciar o procedimento de captura de sinal normalmente.

A detecção de carga de bateria e o alarme de pouca carga foi modelada da forma que, é necessário receber uma entrada com um valor de carga da bateria através do *ComponentType* *InputBatteryCharge*, que deve processar este valor de carga utilizando o *ComponentType* *ProcessBatteryCharge*, e então ser enviado para um supervisor. O supervisor é representado pelo *ComponentType* *Supervisor*, responsável por identificar se a carga da bateria recebida esta abaixo do normal, e caso esteja, impeça que o ECG e os componentes que utilizarão a energia fornecida pela bateria não funcione de forma correta e emita um alarme informando que a bateria está com pouca carga. Caso a bateria possua carga suficiente, deve ser ser informado ao ECG e aos outros componentes a quantidade da carga e que devem funcionar normalmente.

![enter image description here](https://i.imgur.com/puP4a88.png)

Cada *ComponentType* precisa ter um diagrama de composição associado para que as portas de comunicação com as interfaces possam ser definidas. Assim o primeiro ComponentType deifnido chamado de InputBatteryCharge possui uma porta de saída, fornecendo a interface InputBatteryCharge como saída, como é possível observar na imagem abaixo.

![enter image description here](https://i.imgur.com/G6c1pqu.png)

Após ser definido o diagrama de composição para o *ComponentType* *InputBatteryCharge* que prover a interface *InputBatteryCharge*, foi definido um diagrama de composição para o segundo *ComponentType* criado, chamado de *ProcessBatteryCharge*. O diagrama de composição do componente *ProcessBatteryCharge*, possui duas portas de comunicação, a primeira é uma porta de entrada onde o componente precisa da interface *InputBatteryCharge* fornecida pelo componente *InputBatteryCharge*. A partir da interface requerida, o componente *ProcessBatteryCharge* prover a interface *ProcessBatteryCharge*, como é possível observar na imagem abaixo.

![enter image description here](https://i.imgur.com/ha1TVCs.png)

Após a definição das portas do componente *ProcessBatteryCharge*, foi definido a composição do componente Supervisor. O diagrama de composição do componente supervisor possui três portas de comunicação. A primeira porta é requerida a interface *ProcessBatteryCharge*, fornecida pelo componente *ProcessBatteryCharge*. Assim o *ComponentType* Supervisor prover duas interfaces através de duas portas. As interfaces providas pelo *ComponentType* Supervisor são: *AlarmBattery* e *Voltage*. É possível observar o diagrama de composição do *ComponentType* na figura abaixo.

![enter image description here](https://i.imgur.com/jJybeGi.png)

Após definir todos os diagramas de composição para cada ComponentType, nós podemos observar as relações de dependencia e realização criadas no diagrama de classes. No diagrama de classes também podemos observar um componente do tipo *Component* chamado BatterySW. Este componente possui um diagrama de composição, que representa todo o fluxo dos componentes e interfaces do diagrama de classes. No diagrama de composição do BatterySW são utilizados os ComponentImplementations que possui um relacionamento de realização com cada ComponentType associado. O diagrama de composição que representa todo o fluxo da bateria pode ser observado na imagem a seguir.

![enter image description here](https://i.imgur.com/68lAQXU.png)


O diagrama de composição do *BatterySW* inicia-se através do *InputBatteryCharge_impl* recebendo um valor de carga da bateria e enviando esse valor para a porta de saída nomeada do tipo *InputBatteryCharge*. Esta porta de saída possui uma comunicação direta com a porta *in_bc* do mesmo tipo no *PBC_impl*. O *PBC_impl* ao receber o valor através da sua porta de entrada, o *ComponentImplementation* entra em funcionamento e então pode pode retornar uma informação para a sua porta de saída *PBC* do tipo *ProcessBatteryCharge* e que possui comunicação direta com o *Supervisor_impl* através da porta *pb_c*. Dentro do *Supervisor_imp*, ele irá decidir para onde encaminhar a informação, se será um alarme de pouca carga de bateria emitido através da porta *alarm* do tipo *AlarmBattery* ou uma informação para a porta *output_charge* do tipo *Voltage*.


## Eletrocardiograma

No diagrama de classes do eletrocardiograma, foram definidos seis *ComponentTypes*, esses ComponentTypes são: *SetupElectrodes*, *Electrodes*, *Amplifier*, *ApplyFilters*, *Conversor* e ECG. Cada *ComponentTypes* possui um relacionamentos de realização ou dependencia com algumas interfaces definidos através do diagrama de composição de cada, e também cada *ComponentTypes* possui um relacionamento de realização com um *ComponentImplementation*.

O primeiro *ComponentTypes* é *SetupElectrodes*. Este componente representa a configuração dos eletrodos no corpo do paciente e verificação se os eletrodos estão alocados corretamente retornando uma verificação de impedância para o ECG informando que os eletrodos estão configurados de forma correta. Assim o *ComponentType* *SetupElectrodes* possui uma porta que prover a interface *ElectrodeImpedance*. É possível observar o diagrama de composição do *ComponentType* *SetupElectrodes* na imagem a seguir.

![enter image description here](https://i.imgur.com/fTMHiA8.png)

O próximo *ComponentType* é o *ComponentType* *Electrodes*, que em seu diagrama de composição possui duas portas de comunicação. Uma porta de entrada onde requer a interface *ElectrodeImpedance* fornecida pelo *ComponentType* *SetupElectrodes*, e a outra porta de comunicação, onde o *ComponentType* *Electrodes* prover a interface *Signal*. Este componente representa a necessidade da configuração correta dos eletrodos recebendo uma impedância, e então retornando fornecendo o sinal como saída. Pode-se observar diagrama de composição do *ComponentType* *Electrodes* na imagem a seguir.

![enter image description here](https://i.imgur.com/QW9Fa9o.png)

Após os eletrodos, o *ComponentType* *Amplifier* recebe a *Interface* *ElectrodeSignal* que o *ComponentType* *Electrodes* fornece. Assim o componente que representa o amplificador de signal amplifica o sinal e prover a interface *AmplifiedSignal*. Este fluxo é representado em duas portas de comunicação no diagrama de composição do *ComponentType* *Amplifier*, onde a primeira porta requer a *Interface* *ElectrodeSignal* fornecida pelo *ComponentType* *Electrodes*, e a segunda porta fornece a interface *AmplifiedSignal*. Esse diagrama de composição é possível observar na imagem a seguir.

![enter image description here](https://i.imgur.com/DaIDIIi.png)

Após o *ComponentType* *Amplifier* prover a interface *AmplifiedSignal*, o *ComponentType* *ApplyFilters*, possui uma porta de comunicação, onde é requerida a *Interface* *AmplifiedSignal*, fornecida pelo *ComponentType* *Amplifier*. Após receber esta Interface, o *ComponentType* *ApplyFilters* fornece a *Interface* *FilteredSignal*, onde representa a recepção de um sinal, e a aplicação dos filtros de passa baixa, passa alta e passa banda. O diagrama de composição do *ComponentType* *ApplyFilters* pode ser observado na imagem a seguir.

![enter image description here](https://i.imgur.com/EEqo2bR.png)

Após o *ComponentType* *ApplyFilters* fornecer a interface *FilteredSignal*, o sinal segue seu fluxo e agora o *ComponentType* *Conversor* requer em uma de suas portas de comunicação a *Interface* *FilteredSignal*. Após esta *Interface* obtida pelo *ComponentType* *Conversor*, o mesmo agora fornece uma *Interface* chamada de *ConversorSignal*, que representa a conversão do sinal capturado. O diagrama de composição do *ComponentType* *Conversor* pode ser observado na imagem a seguir.

![enter image description here](https://i.imgur.com/PlrJAf2.png)

Após o sinal capturado pelos eletrodos passar por todos esses componentes citados anteriormente dentro do diagrama de classes, o fluxo do sinal dentro do ECG pôde ser representado. Então o ComponentType Conversor prover a Interface ConversorSignal, e assim o último ComponentType deste diagrama que representa o ECG, chamado de ECG possui uma porta de comunicação onde requer a interface ConversorSignal, e então ele retorna o sinal como resultado provendo a interface ECG. Assim, o diagrama de composição do ComponentType ECG pode ser representado na imagem a seguir.

![enter image description here](https://i.imgur.com/NCYu6PP.png)

Com a definição de todos os ComponentTypes e seus diagramas de composição adicionando portas de comunicação, e também a criação dos ComponentsImplementations, onde cada ComponentType possui um com relacionamento de realização, o diagrama de classe para o eletrocardiograma pode ser observado na imagem a seguir.

![enter image description here](https://i.imgur.com/1YMYiYT.png)




![Diagrama de composição para o fluxo do sinal capturado dentro do ECG](https://i.imgur.com/o3PDQnz.png)

No diagrama de composição *ECGSignalSW* foram adicionados os *ComponentsImplementations* onde cada um representa um *ComponentType*. O fluxo se inicia através do *setupElectrodes_impl* que possui uma porta de saída nomeada de *impedance* do tipo *ElectrodeImpedance*. Esta porta fornece a *Interface* *ElectrodeImpedance* para a porta *impedance_in* do *ComponentImplementation* *electrodes_impl*. Assim a porta *signal_out* do tipo *ElectrodesSignal*, realiza uma comunicação direta com a porta de entrada *signal_in* do tipo *ElectrodesSignal* no *ComponentImplementation* *Amplifier_impl*. Dentro do *Amplifier_impl*, ele realiza as operações de amplificação de sinal e então envia o sinal para uma porta de saída que se comunica com a porta de entrada *ampl* do *ApplyFilter_impl*, possuindo o tipo *Amplifier*. O *applyFilter_impl* aplica os filtros de passa baixa, passa alta e passa banda e então utiliza a porta de saída *apply_filter* do tipo *FilteredSignal* para a porta de entrada *appl* possuindo o tipo *FilteredSignal* no *conversor_impl*. O *conversor_impl* realiza a conversão do sinal, e então permite que o sinal utilize sua porta de saída *conversor* do tipo *ConversorSignal* para se comunicar com a porta de entrada *cnv* do tipo *ConversorSignal* do ComponentImplementation *eCG_impl*. O *eCG_impl* por fim possui uma porta de saída nomeada de *ecg* do tipo *ECG* que representa o resultado dos sinais capturados por um ECG.

## Servidor de resultados

Em um ambiente de UTI cardíaca, os pacientes devem ser monitorados. Para facilitiar esse processo de monitoramento, existe um servidor que capta, armazena e prover essas informações para os profissionais que precisam acompanhar os pacientes. Este servidor deve possuir uma tela de resultados com informações dos pacientes, salvar informações do paciente e recupera-las, analisar o sinal em tempo real, e ser capaz de emitir alarmes caso alguma anomalia urgente seja detectada naquele momento. 

Assim, o diagrama de classes para o servidor de resultados contem três ComponentTypes. O primeiro ComponentType chamado de ReceiveSignal, é representa a recepção de um sinal e então prover a interface ReceiveSignal através de uma porta de comunicação criada em seu diagrama de composição que é possível observar na imagem abaixo.

![enter image description here](https://i.imgur.com/9sLFT6U.png)

Um outro *ComponentType* definido para a modelagem do servidor de resultados, é o *ComponentType* *CentralSystem*, que representa um processador e centralizador de informações. Este *ComponentType* requer como entrada em uma de suas portas de comunicação a Interface *ReceiveSignal* e prover como saída a interface *CentralSystem*. Estas duas portas de comunicação do *ComponentType* *CentralSystem* é possível observar na imagem a seguir.

![enter image description here](https://i.imgur.com/zDlbNvl.png)

O terceiro *ComponentType* definido no diagrama de classe para a tela de resultados, foi o *ComponentType* *Stream*. Este ComponentType é resposável por entregar as informações no servidor de resultados para os profissionais do ambiente de UTI que estão acompanhando. No diagrama de composição do ComponentType Stream, existem duas portas de comunicação. A primeira porta requer a interface CentralSystem de entrada, provida pelo ComponentType CentralSystem. Com isso, o ComponentType Stream prover em sua porta de saída a interface Stream, representando os resultados e informações coletados do paciente. Pode-se observar o diagrama de composição para o ComponentType Stream na imagem a seguir.

![enter image description here](https://i.imgur.com/RlzyLbx.png)

Definindo o diagrama de classes para o servidor de resultados com Interfaces, ComponentTypes e seus diagramas de composição, e também os ComponentImplementations, nós podemos observar na imagem a seguir como ficou o diagrama de classes e os relacionamentos de realização e dependencia entre os ComponentTypes e as Interfaces. O diagrama de classes para o servidor de resultados, é possível observa na imagem a seguir.

![enter image description here](https://i.imgur.com/GASUSHC.png)

Para representar todo o diagrama de classes do servidor de resultados em um diagrama de composição, foi necessário definir um *Component* chamado *ScreenResultsSW*. Dentro deste diagrama de composição, são utilizados os ComponentImplementations e as portas aos quais os diagramas de composição que os componentes que estão associados possuem. O diagrama de composição inicia seu processo através do *receiveSignal_impl*, onde possui uma porta de saída *rs* do tipo *ReceiveSignal* em comunicação direta com a porta *rs* no *centralSystem_impl*. A partir do *centralSystem_impl*, existe o envio das informações para a tela de resultados final. Este envio é representado através da comunicação entre as portas *cs* no *CentralSystem_impl* para a porta *c_s* em *stream_impl*, e então essa implementação é responsável por exibir as informações do paciente através da porta *stream* do tipo *Stream*.

![Diagrama de composição](https://i.imgur.com/NZFpfdw.png)



## Visão geral do ECGSW

Esta modelagem é a modelagem de software de maior nível superior. Todas as outras modelagens já desenvolvidas vão ser conectadas através desta modelagem. Para isso, foi necessário referenciar todos os components de nível superior de cada modelagem, ou seja os components resposáveis pelos diagramas de composições.

 1. AlarmSW
 2. ECGSignalSW
 3. DetectDiseasesSW
 4. ScreenResultsSW

![enter image description here](https://i.imgur.com/xRT8t5X.png)

Após adicionar os components de maior nível superior de cada modelagem, e que representam seus  diagramas de composição, foi preciso também criar um component responsável para a modelagem de visão geral do ECG, e através dela poder realizar a conexão das outras modelagens e o diagrama de composição.

![Diagrama de composição de visão Geral do ECG](https://i.imgur.com/86RX9RX.png)

O diagrama de composição da visão geral do ECG representa as saídas que cada modelagem apresenta e possui a função de conectar uma modelagem com a outra. O inicio do fluxo inicia através da modelagem do *AlarmSW*, onde as saídas são um alarme caso a bateria seja baixa, ou uma informação com o valor de carga da bateria que logo é passada para o fluxo de sinal do ECG. A saída do fluxo de sinal do ECG podem ter dois caminhos, o primeiro é, caso alguma anomalia seja detectada, essa informação é enviada para a modelagem que é responsável por tratar doenças cardíacas e assim emitir um alarme para a doença cardíaca detectada, caso não o *ECGSignalSW* pode transmitir o sinal para a modelagem da tela de resultados, que então pode exibir os resultados em uma tela para os profissionais responsáveis por monitorar e analisar.


## Monitor Multiparametrico

A modelagem de um monitor multiparametrico, representa um ambiente de uma UTI de um leito para um paciente. Um monitor multiparametrico em uma UTI é capaz de coletar dados de respiração, temperatura, oxigenação e também dados cardíacos de um paciente (por link do artigo). Assim um monitor multiparametrico possui componentes de ECG para leitura de sinais cardiacos, oximetro de pulso para medir a quantidade de oxigênio no sangue do paciente, termômetro para medir a temperatura do corpo do paciente e também um componente para medir a taxa de respiração. Além desses componentes, um monitor multiparametrico é composto por uma bateria e também por eletrodos para captar as informações e enviar ao monitor multiparametrico.

Com o CHESS essa modelagem foi desenvolvida definindo os componentes do tipo *ComponentType* de Bateria, Eletrodos, Detecção de Doenças, Eletrocardiograma, Oximetro de Pulso, Processamento de Sistema, Servidor de Resultados, Taxa Respiratória, Termomêtro e Resultados. O primeiro passo para a modelagem do monitor multiparametrico foi criar um diagrama de classes onde é possível definir todos os *ComponentTypes*, ComponentImplementations e Interfaces. É possível observar o diagrama de classes para o monitor multiparametrico com todas as *Interfaces*, *CompononentsTypes* e *ComponentsImplementations* definidos na imagem a seguir.

![enter image description here](https://i.imgur.com/vbqszBU.png)


### Bateria
O *componentType* que representa a bateria possui relacionamento com duas interfaces. Os relacionamentos do component que representa a bateria se dão através de duas portas que prover duas interfaces, uma interface que representa o alarme de bateria em momentos que a bateria não possui carga suficiente para operar, e a outra interface é para energia, chamada de *Voltage*, onde caso a bateria possua carga suficiente para operar, a saída se dar através dessa interface para comunicar com os outros componentes que necessitam dessa interface para funcionar.

![enter image description here](https://i.imgur.com/k41j2gF.png)

### Eletrodos

Os eletrodos possui um ComponentType que representa-o. Este ComponentType que é possível observar na Imagem abaixo, possui relacionamento com duas interfaces. O primeiro relacionamento é com a Interface de *Voltage*, onde os eletrodos necessitam da comunicação com a interface de *Voltage* para funcionar perfeitamente fornecida pelo *ComponentType* de bateria, e então os *ComponentType* de eletrodos prover a interface de impedância.

![enter image description here](https://i.imgur.com/lQimz50.png)

### Eletrocardiograma

Para representar o ECG dentro do monitor multiparametrico, foi necessário definir um ComponentTpe chamado de Electrocardiogram e então, definir para este component um diagrama de composição, para assim criar os relacionamentos desse componente com as interfaces. O ComponentType do ECG possui três portas de comunicação. As duas primeiras portas requeridas para o funcionamento do ECG, essas portas possuem comunicação direta com a interface de Voltage e de Impedancia. A interface de Voltage é fornecida através do component da bateria e a interface de impedância é fornecida através do componente dos eletrodos. Com essas duas portas definidas, o componente pode se comportar corretamente e então prover a interface de sinal através da sua porta de saída.

![enter image description here](https://i.imgur.com/l1HMc8I.png)

### Detecção de Doenças

No monitor multiparametrico, é necessário analisar o sinal que o ECG capta para que se possa retornar resultados, e informar se o paciente está passando por alguma anomalia cardíaca ou não. Assim o compononente detecção de doenças responsável por detectar as anomalias cardiacas do paciente, possui duas portas de comunicação. Uma porta ele necessita da interface Sinal fornecida pelo componente ECG, e a outra porta de comunicação, é fornecida a interface doença detectada, como é possível observar na imagem abaixo.

![enter image description here](https://i.imgur.com/bB7ILnp.png)

### Oximetro de Pulso

O oximetro de pulso é representado através do *ComponentType* *PulseOximeter*. Definindo o diagrama de composição deste componente, foram atribuidas três portas de comunicação. A primeira porta é para a interface de Voltage fornecida pelo componente da bateria, é uma porta onde é necessário ter a interface de Voltage para funcionar. As outras duas portas do componente do oximetro de pulso, são portas de saídas, onde o componente de oximetro de pulso prover duas interfaces, que são: medição de pulso e alarme para medição do pulso. O diagrama de composição do oximetro de pulso é possível observar na imagem abaixo.

![enter image description here](https://i.imgur.com/qh9bTRU.png)

### Taxa Respiratória

A taxa respiratória é representada através do componente *RateRespiratory*, onde poderá ser medido a taxa respiratória do paciente e assim essas informações serem processadas e enviadas ao monitor multiparametrico. O diagrama de composição da taxa respiratória é semelhante ao diagrama de composição do oximetro de pulso. São três portas de comunicação, onde uma requer a interface de *Voltage* e as outras duas prover interfaces de alarme para respiração fora do comum, e também prover uma interface com o resultado sobre a respiração do paciente. As interfaces providas são *RespiratoryAlarm* e *RespiratoryFrequency*, como é possível observar na imagem abaixo.

![enter image description here](https://i.imgur.com/IM0ah3E.png)

### Termômetro

O componente para o termometro, possui relacionamento com duas interfaces em seu diagrama de composição. O componente requer a interface de *Voltage* para funcionar semelhante a outros componentes ja mecionados até aqui. O componente de termometro prover uma interface de temperatura para representar a informação de saída que esse componente fornece. Esta descrição do componente termometro pode ser observada na imagem abaixo.![enter image description here](https://i.imgur.com/UZeABor.png)

### Processador do Sistema

Após capturar todas as informações através dos componentes de ECG, termometro, taxa de respiração, oximetro de pulso e etc, o componente processador de sistema é responsável por capturar as informações dos outros componentes e enviar para o monitor multiparametrico mostrar os resultados, e também enviar para um servidor de resultados onde os profissionais da UTI possam acompanhar.

O processador do sistema requer algumas interfaces para que possa funcionar. As interfaces que o processador do sistema requer são: *BatteryAlarm* e *Voltage*, ambas fornecidas pelo componente de bateria. A interface Signal fornecida pelo componente do ECG. PulseMeasurement e PulseAlarm fornecidas pelo componente de Oximetro de Pulso. RespiratoryFrequency e RespiratoryAlarm fornecidas através do componente Taxa Respiratória. Temperature fornecida através do componente Termometro. Ao receber essas interfaces como entrada através das portas criadas para cada interface, o componente processador do sistema deve prosseguir o fluxo e fornecer algumas interfaces como saída, que são: BatteryAlarm, PulseAlarm, RespitaroryAlarm e Results. Essas interfaces de saída devem ser consumidas pelos componentes servidor de resultados e resultados.


![enter image description here](https://i.imgur.com/ipYfamr.png)


### Servidor de Resultados

O componente de servidor de resultados, é responsável por captar as informações providas pelo monitor multiparamétrico e enviar para um servidor de resultados, onde os profissionais como médicos e enfermeiros possam ter acesso, e que também possam receber alarmes caso alguma anomalia esteja acontecendo com o paciente.

O componente requer algumas interfaces, que são: *DiseasesDetected*, provida pelo componente de detecção de doenças. Signal, provida pelo componente ECG. *RespiratoryAlarm* provida pelo componente *RateRespiratory*. *PulseAlarm* provida pelo componente oximetro de pulso. BatteryAlarm provida pelo component de bateria, e por fim a interface *Results* provida pelo componente *ProcessSystem*. Ao receber essas interfaces, o servidor de resultados é capaz de prover a interface *ServerResultsStream*, onde é possível observar na imagem abaixo.

![enter image description here](https://i.imgur.com/ZoZVgWZ.png)

### Resultados

O componente de resultados, é o componente que simula as informações que o monitor multiparametrico é capaz de informar. Este componente recebe informações providas de outros componentes e então prover essas informações através de uma interface definida para os resultados finais.

O componente de resultados em seu diagrama de composição requer uma interface *Results* fornecida pelo componente *ProcessSystem*, e então o componente *Results* fornece a interface *StreamResults*, representando todas as informações coletadas dos outros componentes que o *ProcessSystem* forneceu. O diagrama de composição do *Results* pode ser observado na imagem a seguir.

![enter image description here](https://i.imgur.com/XXj50gr.png)

Após definir todos os diagramas de composição para cada *ComponentType*, atribuindo as *Interfaces* nas portas, e então possuindo as relações de realização e dependência como é possivel observar na imagem do diagrama de classes, é necessário criar um *Component* sem esteriótipo, e então nele criar um diagrama de composição que represente todo o fluxo de informações de um monitor multiparametrico definido no diagrama de classes. Dentro do diagrama de composição para o monitor multiparamétrico, será utilizado todos os *ComponentImplementations* definidos no diagrama de classes, e que possuem relações com cada *CompononentType*.

O *Component* criado para definir o diagrama de composição é o component chamado de General_ICU. É possível observar a definição dele no diagrama de classes na cor cinza. Na imagem a seguir, é possível observar o diagrama de composição para o *Component* General_ICU, contendo os *ComponentImplementations*, as portas definidas em cada *ComponentType* e seus tipos. No diagrama de composição utilizando os *ComponentImplementations*, foi definido os conectores entre as portas para representar a comunicação entre os compononentes e as portas.

![enter image description here](https://i.imgur.com/oWJok9q.png)