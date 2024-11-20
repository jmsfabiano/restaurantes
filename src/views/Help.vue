<template>
  <div class="profile">
    <Navbar :navbarData="navbarData" />
    <Sidebar activePage="help" />
    <h3 class="bold-700 title">Dúvidas Frequentes</h3>
    <div class="content container-data">
      <h4>Bem-vindo à página de ajuda! Clique nos tópicos abaixo para ver as respostas.</h4>
      <div class="accordion-container">
        <div v-for="(section, sectionIndex) in faqSections" :key="sectionIndex" class="accordion-item">
          <div class="accordion-header" @click="toggleAccordion(sectionIndex)">
            <h5>{{ section.title }}</h5>
            <img :src="arrowIcon" :class="{ 'rotate': section.open }" class="arrow-icon" />
          </div>
          <div class="accordion-body" v-show="section.open">
            <div v-for="(item, itemIndex) in section.items" :key="itemIndex">
              <div class="accordion-item">
                <div class="accordion-header" @click="toggleItemAccordion(sectionIndex, itemIndex)">
                  <h5>{{ item.question }}</h5>
                  <img :src="arrowIcon" :class="{ 'rotate': item.open }" class="arrow-icon" />
                </div>
                <div class="accordion-body" v-show="item.open">
                  <p v-html="item.answer"></p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>

import { ref } from 'vue';
import Navbar from '../components/Navbar.vue';
import Sidebar from '../components/Sidebar.vue';
import arrowIcon from '@/assets/img/arrow_drop_down.svg';

const navbarData = {};
const sidebarData = {};

const faqSections = ref([
  {
    title: `Configurações da Conta`,
    open: false,
    items: [
      {
        question: `Quais são as taxas cobradas pela Prattu?`,
        answer: `Na Prattu, cobramos uma mensalidade fixa de <b>R$ 179,99</b> para os restaurantes que utilizam nossa plataforma. Além disso, aplicamos uma taxa de processamento de <b>3,6%</b> para compras realizadas no <b>crédito</b> e de <b>R$1,19</b> para compras feitas através do <b>Pix</b>.<br>
                 Essas taxas são competitivas e até <b>75% menores</b> quando comparadas a outras empresas do setor. Nossa estrutura de tarifas foi desenhada para ser justa e acessível, visando proporcionar o máximo de benefício aos nossos parceiros sem comprometer suas margens de lucro.`,
        open: false
      },

      {
        question: `Como altero a pessoa responsável pelo restaurante?`,
        answer: `Acesse a página de "Configurações Gerais" no dashboard e atualize os dados do responsável.`,
        open: false
      },

      {
        question: `Esqueci minha senha. Como posso recuperá-la?`,
        answer: `Na página de login, clique em "Esqueci minha senha" e siga as instruções para redefini-la.`,
        open: false
      },

      {
        question: `É possível desativar temporariamente meu restaurante na plataforma?`,
        answer: `Infelizmente ainda não oferecemos essa funcionalidade aos estabelecimentos. Entretanto, caso tenha algum problema, sinta-se à vontade para nos enviar uma mensagem através da aba “Suporte”. Ficaremos felizes em ajudar!`,
        open: false
      },

      {
        question: `Como faço para excluir a minha conta da plataforma?`,
        answer: `Entre em contato conosco através da aba <b>“Suporte”</b> e te auxiliaremos no processo de exclusão de conta.`,
        open: false
      },

      {
        question: `Como adiciono ou removo administradores da conta do restaurante?`,
        answer: `Ainda não estamos oferecendo essa funcionalidade aos estabelecimentos. O acesso precisa ser feito única e exclusivamente através da conta criada pelo administrador.`,
        open: false
      },

      {
        question: `Posso configurar diferentes níveis de acesso aos meus funcionários?`,
        answer: `Estamos planejando oferecer essa funcionalidade em breve para os estabelecimentos. No momento, o acesso precisa ser feito única e exclusivamente através da conta criada pelo administrador.`,
        open: false
      },

      {
        question: `Posso fazer parte de mais de uma categoria (Ex: doceria e cafeteria)?`,
        answer: `Não, você só pode fazer parte de <b>uma categoria</b> na Prattu. É importante escolher a categoria correta porque isso influencia diretamente na busca dos usuários na plataforma. Categorias precisas garantem que seu restaurante apareça nas pesquisas relevantes, aumentando a visibilidade e atraindo o público certo.`,
        open: false
      },

      {
        question: `Como configuro a disponibilidade de horário do meu estabelecimento?`,
        answer: `Para configurar a disponibilidade de horário do seu estabelecimento, acesse o dashboard da Prattu e <b>vá para a seção "Horário de Funcionamento"</b>. Lá, você poderá definir os horários de funcionamento e os dias da semana em que seu restaurante estará aberto. `,
        open: false
      },

      {
        question: `Como funciona a configuração do tempo médio de preparo? Posso ajustar quantas vezes eu quiser?`,
        answer: `A configuração do tempo médio de preparo é essencial para manter os clientes informados sobre quanto tempo esperar pelo pedido. O tempo que o estabelecimento configurar na aba de “Tempo de Preparo” <b>será o tempo que ele terá para preparar os pedidos</b>, bem como o tempo que será mostrado para o usuário no aplicativo.<br>
                 Recomendamos definir um tempo viável e não muito longo, para evitar que os consumidores desistam da compra.
                 Esse tempo pode ser ajustado quantas vezes for necessário.`,
        open: false
      },

      {
        question: `Quais são as formas de pedidos aceitas pela Prattu?`,
        answer: `Aceitamos duas formas de pedido: <b>take-out</b> (pedidos para retirada) e <b>dine-in</b> (pedidos para consumir no local). 
                 Vale lembrar que cada restaurante pode configurar as formas de pedido aceitas e não necessariamente precisam trabalhar com as duas (take-out e dine-in).`,
        open: false
      },

      {
        question: `Como respondo a avaliações de clientes?`,
        answer: `No momento, a Prattu não oferece a funcionalidade de responder diretamente às avaliações dos clientes. No entanto, <b>você pode visualizar</b> todas as avaliações recebidas acessando a seção "Eficiência Operacional" no dashboard do seu restaurante. Aqui, você verá o feedback detalhado dos clientes, permitindo que você entenda melhor suas experiências e identifique áreas para melhoria. <br>
                 Use essas informações para ajustar seus serviços e produtos, garantindo uma melhor satisfação dos clientes no futuro. Fique atento às atualizações da plataforma, pois essa funcionalidade poderá ser disponibilizada em breve.`,
        open: false
      },
    ]
  },
  {
    title: `Cardápio e Adição de Itens`,
    open: false,
    items: [
      {
        question: `Como adiciono novos itens ao meu cardápio?`,
        answer: `Para adicionar novos itens ao seu cardápio, acesse o dashboard e vá até a seção "Cardápio". Clique em "Adicionar Item" e preencha as informações necessárias, como nome do item, descrição, preço e categoria. Não se esqueça de adicionar uma imagem atraente para cada item.`,
        open: false
      },

      {
        question: `Posso criar categorias para meus itens?`,
        answer: `<b>Sim, você pode e deve</b> criar categorias para organizar melhor o seu cardápio. Categorias ajudam os clientes a encontrarem os itens de maneira mais fácil e rápida. Para criar uma categoria, vá até "Cardápio" e selecione "Adicionar Categoria".`,
        open: false
      },

      {
        question: `Posso criar mais de um cardápio para o meu estabelecimento?`,
        answer: `Sim, você poderá criar mais de um cardápio e configurá-los para diferentes horários do dia. Por exemplo, um cardápio ficará ativo no almoço, das 11h às 15h, e outro no jantar, das 19h às 00h.`,
        open: false
      },

      {
        question: `Quantas categorias posso adicionar ao meu cardápio?`,
        answer: `Não há limite para o número de categorias que você pode adicionar. É importante escolher as categorias corretas, pois isso influenciará a busca dos usuários na plataforma e facilitará a navegação pelo seu cardápio.`,
        open: false
      },

      {
        question: `Como edito ou removo um item do meu cardápio?`,
        answer: `Para editar ou remover um item, acesse a seção "Cardápio" no dashboard, encontre o item que deseja modificar e clique em "Editar" ou "Remover". Faça as alterações necessárias e salve.`,
        open: false
      },

      {
        question: `Posso adicionar descrições e imagens aos itens do cardápio?`,
        answer: `Sim, adicionar descrições detalhadas e imagens de alta qualidade é altamente recomendado. Isso ajuda a atrair os clientes e fornecer informações claras sobre o que eles estão pedindo.`,
        open: false
      },

      {
        question: `Como configuro variações de itens (tamanhos, sabores, adicionais etc.)?`,
        answer: `Na seção "Adicionar Item" ou em “Complementos”, você pode configurar variações e opções adicionais para cada item. Por exemplo, para um Açai, você pode adicionar variações de tamanho (pequeno, médio, grande) e opções de adicionais (extra morango, leite condensado).`,
        open: false
      },

      {
        question: `Como ajusto os preços dos itens do meu cardápio?`,
        answer: `Para ajustar os preços, vá até a seção "Cardápio" no dashboard, encontre o item cujo preço deseja alterar, clique em "Editar" e atualize o preço conforme necessário.`,
        open: false
      },

      {
        question: `Como configuro a disponibilidade de itens (disponível/indisponível)?`,
        answer: `Se um item estiver temporariamente indisponível, você pode desativá-lo na seção "Cardápio". Basta encontrar o item, clicar em "Editar" e marcar como "Indisponível". Você pode reativá-lo a qualquer momento.`,
        open: false
      },

      {
        question: `Como gerencio a ordem dos itens no meu cardápio?`,
        answer: `Você pode arrastar e soltar os itens na seção "Cardápio" para organizar a ordem conforme desejar. Isso ajuda a destacar itens populares ou promocionais na parte superior do seu cardápio.`,
        open: false
      },

    ]
  },
  {
    title: `Gerenciamento de Pedidos`,
    open: false,
    items: [
      {
        question: `Como recebo notificações de novos pedidos?`,
        answer: `As notificações de novos pedidos são enviadas automaticamente para o seu dashboard e para o e-mail cadastrado. Caso queira alterar o e-mail cadastrado, acesse a aba de “Configurações Gerais”`,
        open: false
      },
      {
        question: `O que devo fazer se não puder aceitar um pedido?`,
        answer: `Na página “Pedidos”, vá até o pedido específico e selecione a opção "Recusar", especificando o motivo da recusa. A sua explicação será mostrada ao consumidor no aplicativo.`,
        open: false
      },
      {
        question: `Como gerencio pedidos cancelados?`,
        answer: `Todos os pedidos cancelados aparecem na seção de "Histórico de Pedidos" do dashboard. Você pode ver todo o histórico e os motivos dos cancelamentos.`,
        open: false
      },
      {
        question: `Como gerencio pedidos agendados?`,
        answer: `Os pedidos agendados são gerenciados através do <b>gestor de pedidos</b>, sendo diferenciados dos demais por um emoji de “calendário” e uma cor roxa.
                 Ao aceitar um pedido agendado, <b>você configurará o tempo necessário para prepará-lo</b>. O sistema notificará seu estabelecimento na hora certa para iniciar o preparo conforme o tempo configurado por você.
                 Por exemplo, se um pedido foi agendado para amanhã às 14h e você precisa de 1h para prepará-lo, o sistema notificará seu restaurante às 13h para iniciar o preparo. Isso garante que o pedido esteja pronto no horário agendado, proporcionando uma melhor experiência para o cliente.`,
        open: false
      },
      {
        question: `O que acontece caso eu não consiga entregar o pedido no horário?`,
        answer: `Se você não conseguir entregar o pedido no horário estipulado, isso pode resultar em insatisfação do cliente. Clientes insatisfeitos são mais propensos a deixar avaliações negativas, o que <b>pode afetar a reputação do seu restaurante na plataforma</b>.<br>
                 Além disso, a visibilidade do seu estabelecimento nas buscas poderá ser reduzida, tornando mais difícil atrair novos pedidos. Se atrasos na entrega se tornarem recorrentes, <b>a Prattu avaliará a situação e poderá aplicar penalidades</b> para garantir a qualidade do serviço oferecido aos nossos usuários.`,
        open: false
      },
      {
        question: `Como funcionam as colunas “Pendentes”, “Em preparação” e “Pronto para retirada”, na página de “Pedidos”?`,
        answer: `Essas três colunas foram criadas para ajudar os estabelecimentos a organizarem os pedidos de forma eficiente:
                 - <b>Pendentes</b>: esta coluna exibe todos os pedidos que estão aguardando aprovação. Aqui, os estabelecimentos podem revisar e aceitar ou recusar os pedidos.
                 - <b>Em preparação</b>: após a aprovação, os pedidos são movidos para esta coluna. Um cronômetro será exibido, indicando o tempo restante para a conclusão do preparo do pedido. Isso ajuda a gerenciar o tempo e garantir que os pedidos sejam preparados de forma oportuna.
                 - <b>Pronto para retirada</b>: quando o pedido estiver finalizado, ele será movido para esta coluna. O cliente será notificado de que seu pedido está pronto para retirada. Após o cliente retirar o pedido, o restaurante deve marcá-lo como “Pedido retirado”. Os detalhes dos pedidos retirados podem ser consultados na página de “Histórico de pedidos”.
                 Esta organização visual e funcional facilita o gerenciamento dos pedidos, garantindo que cada etapa do processo seja clara e eficiente.`,
        open: false
      },
      {
        question: `Meu estabelecimento está cheio e preciso de um tempo extra para preparar os pedidos. Como configuro isso?`,
        answer: `O seu estabelecimento pode configurar um tempo extra de duas formas diferentes: através do botão <b>“Ajustar tempo de preparo”</b>, localizado no canto superior direito da tela de “Pedidos”, e após aceitar o pedido, através do botão <b>“Adicionar tempo”</b>.
                 O botão no canto superior da tela serve para ajustar o tempo de preparo de <b>todos</b> os pedidos, ou seja, o restaurante poderá adicionar <b>até 10 minutos extras</b> sobre o tempo de preparo padrão pré-configurado.
                 Caso o estabelecimento opte por utilizar o botão “Adicionar tempo”, localizado dentro do card de cada pedido, o tempo será acrescido <b>somente</b> sobre o pedido em específico, e não em todos.
                 Uma alternativa para o estabelecimento é pausar o recebimento de pedidos por um tempo determinado, através do botão <b>“Pausar recebimento de pedidos”</b>, localizado no canto superior direito da tela, também na página de “Pedidos”.`,
        open: false
      },
      {
        question: `Vocês trabalham com algum tipo de integração com PDVs?`,
        answer: `Atualmente, ainda não oferecemos integração com sistemas de PDV (Ponto de Venda). No entanto, estamos trabalhando para disponibilizar essa funcionalidade em breve. Nossa equipe está empenhada em facilitar a gestão do seu restaurante e tornar o processo de vendas ainda mais eficiente e integrado. Fique atento às nossas atualizações para novidades sobre essa funcionalidade.`,
      }
    ]
  },
  {
    title: `Pagamentos e Repasses`,
    open: false,
    items: [
      {
        question: `Como e quando recebo os pagamentos?`,
        answer: `Os pagamentos, para compras feitas no crédito, são realizados a cada 32 dias(D + 32) pelo Asaas, nosso parceiro financeiro.Para compras no Pix, os pagamentos são repassados no momento da aprovação da compra.
                 Você pode visualizar seu saldo, faturamento, pagamentos já feitos, em aberto etc.diretamente na nossa plataforma, acessando a página de “Pagamentos” no dashboard.
                 Entretanto, para efetuar os saques, você deve acessar a plataforma do Asaas através <b><a href="https://www.asaas.com/login/auth?customerSignUpOriginChannel=HOME" target="_BLANK">deste link</a></b>.`,
        open: false
      },
      {
        question: `Posso mudar a conta bancária para recebimento dos pagamentos ?`, 
        answer: `Sim, você pode alterar suas informações bancárias acessando sua conta cadastrada no Asaas através <b><a href="https://www.asaas.com/login/auth?customerSignUpOriginChannel=HOME" target="_BLANK">deste link</a></b>.`,
        open: false
      },
      {
        question: `Quais são as informações financeiras que consigo acessar através do dashboard da Prattu ?`, 
        answer: `Na aba de “Pagamentos” do dashboard, você pode acessar informações como: descrição das cobranças, valor previsto de recebimento, valor confirmado, valor sob custódia, e valor vencido.
                   Para efetuar saques, solicitar adiantamentos ou alterar seus dados financeiros, acesse sua conta no Asaas através <b><a href="https://www.asaas.com/login/auth?customerSignUpOriginChannel=HOME" target="_BLANK">deste link</a></b>.`,
        open: false
      },
      {
        question: `Esqueci minha senha de acesso ao Asaas.O que eu faço ?`, 
        answer: `Se você esqueceu sua senha de acesso ao Asaas, acesse <b>este link</b> e clique em “Esqueci minha senha”. Você receberá instruções no e - mail cadastrado para redefini - la.`,
        open: false
      },
      {
        question: `Posso antecipar o recebimento dos repasses ? Preciso pagar alguma taxa adicional ? `, 
        answer: `Sim, você pode realizar a antecipação do recebimento dos repasses.Para isso, <b>o Asaas cobra uma taxa de 2,10%</b> e, para que a antecipação seja liberada, você precisa: <br>
                   - Estar com o cadastro aprovado no Asaas;<br>
                   - Ter concluído a sua primeira transferência bancária TED ou Pix pela plataforma;<br>
                   - Os dados do cliente da cobrança devem estar preenchidos com nome, CPF ou CNPJ e telefone;<br>
                   - A data prevista da compensação da cobrança não pode ser menor do que 8 dias úteis para cobrança avulsa ou vencimento da última parcela em caso de parcelamento.<br>
                   O prazo de análise é de dois dias úteis e pode ser acompanhado pela plataforma do Asaas.`,
        open: false
      },
      {
        question: `Estou tendo dificuldades em relação à plataforma do Asaas. Quem eu devo contatar?`,
        answer: `Se você estiver enfrentando dificuldades com a plataforma do Asaas, entre em contato com o <b>suporte do Asaas</b> diretamente através do suporte disponível na plataforma, acessando <b>este link</b> ou pelo telefone de atendimento.`,
        open: false
      },
      {
        question: `Quem deve fazer a emissão da nota fiscal, nós do estabelecimento, ou a Prattu?`,
        answer: `<b>O estabelecimento é responsável por emitir a nota fiscal para os clientes</b>. A 
                 Prattu se isenta de qualquer obrigação relacionada à emissão de notas fiscais para o consumidor final.`,
        open: false
      },
      {
        question: `Quais são as notas fiscais que a Prattu emite?`,
        answer: `Ao final de todo o mês, a Prattu emitirá duas notas fiscais aos estabelecimentos: <br>`,
        answer: `- <b>Nota Fiscal de Licenciamento de Software</b>: esta nota refere-se à cobrança mensal de R$179,99 pelo uso da nossa plataforma.<br>
                 - <b>Nota Fiscal de Taxas de Intermediação</b>: esta nota cobre a soma das taxas de intermediação cobradas sobre cada transação realizada na nossa plataforma.<br>`,
        open: false
      },
      {
        question: `Posso realizar saques através da Prattu?`,
        answer: `Não, os saques devem ser realizados acessando sua conta na plataforma do Asaas através <b><a href="https://www.asaas.com/login/auth?customerSignUpOriginChannel=HOME" target="_BLANK">deste link</a></b>.`,
        open: false
      },
      {
        question: `Quais são as formas de pagamento aceitas?`,
        answer: `As formas de pagamento que disponibilizamos aos consumidores finais são: cartão de crédito (à vista) e Pix.`,
        open: false
      },
      {
        question: `Posso configurar um valor para pedido mínimo?`,
        answer: `No momento, não oferecemos a possibilidade de configurar um valor mínimo para pedidos. Os clientes podem realizar pedidos de qualquer valor na Prattu.`,
        open: false
      },

    ]
  },
  {
    title: `Promoções e Cupons`,
    open: false,
    items: [
      {
        question: `Como crio uma promoção para minha loja?`,
        answer: `Vá para a página de "Promoções" no dashboard, escolha a promoção desejada, clique em "Criar Promoção" e siga as instruções para definir os detalhes da promoção.`,
        open: false
      },
      {
        question: `Quais são os tipos de promoções que posso configurar para o meu estabelecimento? Como funciona?`,
        answer: `Na Prattu, você pode configurar diversos tipos de promoções, incluindo:<br>
                 - <b>Descontos em produtos específicos</b>: ofereça descontos em itens selecionados do menu.<br>
                 - <b>Promoções em todos os produtos</b>: crie descontos para todos os produtos em sua loja.<br>
                 - <b>Cupons de desconto</b>: gere cupons que os clientes podem usar em suas compras.<br>
                 Para configurar uma promoção, vá para a seção "Promoções" no dashboard, clique em "Criar Promoção" e siga as instruções para definir os detalhes, como os produtos incluídos, a duração da promoção e o desconto aplicado.`,
        open: false
      },
      {
        question: `Posso editar uma promoção ativa?`,
        answer: `Sim, você pode editar promoções ativas a qualquer momento acessando a seção de "Promoções" e selecionando a promoção que deseja editar.`,
        open: false
      },
      {
        question: `Como crio um cupom de desconto para oferecer aos meus clientes?`,
        answer: `Para criar um cupom de desconto, acesse a página de “Promoções” e, na parte de “Cupom de desconto”, clique em “Criar promoção”.Siga todos os passos e seu cupom estará prontinho para ser usado pelos seus clientes.`,
        open: false
      },
    ]
  },
  {
    title: `Suporte Técnico`,
    open: false,
    items: [
      {
        question: `O que faço se encontrar um problema técnico na plataforma?`,
        answer: `Entre em contato com nosso suporte técnico através da página “Suporte" no dashboard ou envie um e-mail para suporte@prattuapp.com.br`,
        open: false
      },
      {
        question: `Como melhoro minha classificação na plataforma?`,
        answer: `Para melhorar sua classificação na Prattu, considere as seguintes estratégias:<br>
                 - <b>Excelência no atendimento ao cliente</b>: garanta que os pedidos sejam preparados corretamente e entregues dentro do tempo estimado.<br>
                 - <b>Qualidade dos produtos</b>: mantenha um alto padrão de qualidade para todos os itens do seu menu.<br>
                 - <b>Promoções e descontos</b>: ofereça promoções atrativas para incentivar mais pedidos e fidelizar clientes.<br>
                 - <b>Atualizações frequentes</b>: Mantenha seu perfil atualizado com as últimas informações, menus e horários.<br>
                 Com essas estratégias, você poderá aumentar a satisfação dos clientes e melhorar sua classificação na plataforma, atraindo mais usuários e aumentando suas vendas.`,
        open: false
      },
      {
        question: `Como faço para cadastrar meu restaurante na Prattu?`,
        answer: `Para cadastrar seu restaurante, acesse nosso site, crie uma conta e preencha todas as informações solicitadas. Assim que preenchido, nosso time vai levar de 24h a 48h para analisar todas as informações e liberar o acesso do seu estabelecimento.`,
        open: false
      },
      {
        question: `Quais documentos são necessários para o cadastro?`,
        answer: `No momento do cadastro, você deve ter em mãos os seguintes documentos:<br>
                 - Contrato social ou Estatuto da empresa;<br>
                 - Certidão de regularidade do CNPJ;<br>
                 - Documento de identificação dos sócios (RG, CPF ou CNH);<br>
                 - Comprovante de endereço da empresa;<br>
                 - Comprovante de endereço dos sócios e/ou representantes legais;<br>
                 - Inscrição Estadual;<br>
                 - Alvará de funcionamento.`,
        open: false
      },
      {
        question: `Posso alterar as informações do meu restaurante depois de cadastrado?`,
        answer: `Sim, você pode atualizar as informações do seu restaurante acessando as seções “Configurações da Conta” e “Perfil”.<br>
                 Para alterações relacionadas à parte financeira, basta acessar o Asaas, nossa plataforma parceira responsável pela realização de todos os repasses, através do link <b><a href="./perfil" target="_BLANK">deste link</a></b>.`,
        open: false
      },
      /*
      {
        question: `Quais são os CNAEs aceitos?`,
        answer: `Para que o seu restaurante seja aprovado na Prattu, é preciso ter um CNAE das classes 5611, 5612, 5620 e 47211.<br>
                 CNAE 5611: Bares e outros estabelecimentos especializados em servir bebidas, com entretenimento;
                 CNAE 5612: Serviços ambulantes de alimentação;<br>
                 CNAE 5620: Fornecimento de alimentos preparados preponderantemente para consumo domiciliar.<br>
                 CNAE 47211: Comércio varejista de produtos de padaria, laticínio, doces, balas e semelhantes.`,
        open: false
      },
      */
      {
        question: `Quais são as taxas cobradas pela Prattu?`,
        answer: `Na Prattu, cobramos uma mensalidade fixa de <b>R$ 179,99</b> para os restaurantes que utilizam nossa plataforma. Além disso, aplicamos uma taxa de processamento de <b>3,6%</b> para compras realizadas no <b>crédito</b> e de <b>R$1,19</b> para compras feitas através do <b>Pix</b>.<br>
                 Essas taxas são competitivas e até <b>75% menores</b> quando comparadas a outras empresas do setor. Nossa estrutura de tarifas foi desenhada para ser justa e acessível, visando proporcionar o máximo de benefício aos nossos parceiros sem comprometer suas margens de lucro.`,
        open: false
      },
      {
        question: `Quais são os benefícios e vantagens que meu estabelecimento terá ao operar com a Prattu?`,
        answer: `A Prattu oferece inúmeros benefícios aos estabelecimentos, bem como:<br>
                 - Aumento da visibilidade e do alcance do estabelecimento;<br>
                 - Taxas até 75% menores quando comparadas aos principais players do mercado;<br>
                 - Melhora na experiência de compra dos clientes;<br>
                 - Maior lucratividade;<br>
                 - Maior controle dos seus clientes;<br>
                 - Melhor gestão das suas operações;<br>
                 - Suporte técnico personalizado para o seu negócio.`,
        open: false
      },
    ]
  },
]);

function toggleAccordion(index) {
  faqSections.value[index].open = !faqSections.value[index].open;
}

function toggleItemAccordion(sectionIndex, itemIndex) {
  faqSections.value[sectionIndex].items[itemIndex].open = !faqSections.value[sectionIndex].items[itemIndex].open;
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Red+Hat+Text:wght@400&display=swap');

.help.container-data {
  display: flex;
}

.content {
  flex-grow: 1;
  padding: 40px;
  font-family: 'Red Hat Textsans-serif';

}

h3.title {
  font-size: 23px;
  margin-top: 12px;
  margin-bottom: 21px;
  display: inline-block;
}

h4 {
  font-size: 16px;
  font-weight: normal;
  margin-bottom: 20px;
  text-align: left;
  line-height: 26px;
}

.accordion-container {
  margin-top: 20px;
}

.accordion-item {
  margin-bottom: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  overflow: hidden;
}

.accordion-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background-color: white;
  cursor: pointer;
}

.accordion-header h5 {
  margin: 0;
  font-size: 16px;
}

.accordion-body {
  padding: 15px;
  background-color: #fff;
}

.accordion-body p {
  margin: 0;
}

.arrow-icon {
  width: 24px;
  height: 24px;
  transition: transform 0.2s ease;
  transform: rotate(180deg);
}

.rotate {
  transform: rotate(0deg);
}

b {
  font-weight: 600 !important;
}

a {
  color: #000 !important;
  font-weight: 600 !important;
}
</style>