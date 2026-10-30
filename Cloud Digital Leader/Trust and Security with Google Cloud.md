## Modelos de segurança
O **Privileged Access Security Model** concentra privilégios de partes sensíveis em apenas alguns usuários, como os Admins. Entretanto, é importante fazer o gerenciamento dessas permissões com muito cuidado.

O **Least Privilege Security Principle** é sobre garantir acesso apenas às funcionalidades que serão utilizadas pelo usuário e no momento que serão utilizadas. Isso modulariza e impede que setores acessem dados ou performem ações de outros, por exemplo.

A **Zero-Trust Architecure** assume que todos os usuários estão sempre sob suspeição. Isso significa que eles sempre deve ser autenticado e autorizado a interagir e acessar recursos.
## Proteção contra ataques
O **Security By Default Principle** prega que as boas práticas de segurança devem ser implementadas desde o começo das implementações, garantindo uma boa fundação em termos de segurança.

**Security Posture** é o status geral de um ambiente cloud. É isso que normalmente indica o quão bem preparada uma empresa está, com relação aos seus sistemas, políticas e práticas.

**Cyber Resilience** se refere à habilidade real da empresa de resistir e se recuperar de cyber ataques. É sobre identificar, atuar e mitigar riscos, e se recuperar com rapidez.

## Medidas Essenciais de Segurança
O **Firewall** age como um árbitro de acesso à rede, impedindo o acesso de estranhos aos bancos de dados ou sistema.

**Decryption** é o processo de "desfazer" uma encriptação, ou seja, de utilizar a chave para acessar o dado encriptado. Assegurar que essas chaves estão seguras e que elas não caiam na mão de pessoas ou computadores maliciosos é essencial.

**Data Loss Prevention**, ou **DLP**, são estratégias para impedir ou evitar que dados sensíveis sejam perdidos, mal usados ou acessados por usuários não autorizados. Soluções DLP monitoram, identificam e protegem esses dados automaticamente.

**CIA: Confidentiality, Integrity & Availability**
**Confidentiability** trata de manter as informações de forma segura e secreta e Criptografia (Encryption) desempenha um papel essencial nessa frente.
**Integrity** trata de ter os dados precisos e confiáveis, o que deve ser priorizado durante todo o ciclo de vida deles. Checksums ou Digital Signatures são ferramentas de integridade.
**Availability** trata de ter sempre os dados disponíveis quando necessário, o que normalmente é garantido através do uso da Cloud, que por sua vez possui redundâncias e backups que minimizam o downtime e maximizam a possibilidade de acesso.

**Control** são as políticas e garantias técnicas, como um sistema robusto de autenticação, que impõe controle de acesso aos dados e mitiga riscos de segurança.
**Compliance** é o firme cumprimento às leis, regulações e padrões de indústria. Isso minimiza a possibilidade de problemas legais e mostra a stakeholders um compromisso da empresa.

## Identidade
**Three As**: Authentication, Authorization & Auditing

**Authentication** é o guarda na porta, que verifica a identidade de quem deseja acesso.
**Authorization** garante que, após a entrada, as permissões do usuário são respeitadas.
**Auditing**, ou Accounting, monitora e gera logs de cada interação para verificação.

O **IAM**, **Identity and Access Management**, faz gestão dos 3 As no Google Cloud. É uma solução centralizada da nuvem do Google, que concentra de forma eficiente todo tipo de permissionamento.

## **CapEx vs OpEx**
**CapEx** significa **Capital Expenditure**, que mede o custo de pagar por hardware e sistemas de segurança à vista, antes mesmo de ter um produto ou serviço rodando. O advento da Cloud fez com que grande parte dos recursos migrassem de CapEx para **OpEx**, **Operating Expenditure**. Isso significa pagar pelo hardware e pelos serviços de segurança sob demanda, ou por um processo de assinatura.

## **Defense in Depth**
Diversas camadas em partes e etapas diferentes para fornecer boa segurança em todo o sistema. Essa abordagem evita um único ponto de falha.

### Data Centers
**PUE**, Power Usage Effectiveness -> Índice de Eficiência Energética
**Titan Chip** -> Verifica a integridade dos servidores e dos códigos que rodam.

### Secure Storage
Como a criptografia protege os dados em todos os seus possíveis estados:

**At Rest**: quando os dados estão parados, armazenados em dispositivos físicos. Criptografia é padrão, mas podemos gerenciar as chaves de forma individual e opcional com o Cloud Key Management Service (Cloud KMS). **AES-256**

**In Transit**: quando os dados estão se movendo entre redes ou na internet. Criptografia é essencial, pois muitos ataques ocorrem interceptando esses ataques. Quando em trânsito na Cloud, os dados são  encriptados e autenticados em diversas camadas. **TLS (Transport Layer Security)**

**In Use**: quando os dados estão, de fato, sendo usados, por algum serviço e em algum computador. A criptografia nesse caso garante que apenas pessoas autorizadas consigam ler e usar os dados. É usada uma técnica chamada Memory Encryption.

**In Memory**: quando em memória, é usado um hardware especial, completamente fechado, para as aplicações. Isso está disponível para VMs, GKE, Dataflow, etc

### Network Security
O IAP, **Identity-Aware Proxy**, garante que toda requisição pode ser validada com informações de identidade e contexto., e é uma alternativa às VPNs.
Para redes **híbridas ou multicloud,** é utilizado o **Cloud VPN e o Cloud Interconnect.**
Para segmentar e proteger os perímetros, são utilizados firewalls e **o VPC**.
**Google Cloud Armor é a ferramenta para defender a núvem de ataques DDosS.**
O **Certificate Manager** trata dos certificados ciptográficas via web, como TLS/SSL.
### SecOps
Security Operations trata da proteção de uma organização na nuvem. No gcp:
**Security Command Center** -> centralized view
**Cloud** **Logging** -> analise de logs de segurança
**SOAR** (Orchestration, Automation & Response) -> plataforma de automação para detectar e automatizar respostas a ataques

### Cybersecurity Threats
**Social Engineering** -> Quando atacantes usam truques sociais para enganar funcionários de dentro do sistema e adquirir acesso de forma deceptiva. Pishing Attacks.
**Physical Damage** -> Dano a componentes físicos dos servidores.
**Malware, Virus, Ramsomware** -> O pior deles é o ramsomware, que deixa dados e arquivos como reféns de pagamento externo.
**3rd Party Systems** -> depender de funções de sistemas de fora para  funções essenciais, como financeiro e invantário, pode ser prejudicial se medidas cautelares de segurança não forem colocadas em prática.
**DDoS** -> Distributed Denial of Service, computadores infectados compõem tráfico ilegítimo na rede, e impede que usuários legítimos tenham acesso aos serviços.

### Solutions & Help
**Frontline Intelligence** -> Atendentes e consultores da Mandiant apontam insights imediatos para a ameaça de segurança e seus possíveis impactos.
**Crowdsorced Intelligence**  -> O Google Threat Intelligence coleta dados de diversos usuários e usa de bancos de dados como o VirusTotal e tira insights sobre eles.
**OSINT** -> Open Source Intelligence, uso de bancos de dados públicos para extrair insights.
**Human-curated Intelligence** -> Dados selecionados e tratados por profissionais experientes, agregados pela Mediant. Funciona bem para casos específicos ou padrões comportamentais de ameaças.
**Google Insights** -> Telemetria de segurança que o próprio google coleta do seu ecossistema de produtos e serviços.

### AI Security Offerings
**Disponíveis através do Security Command Center:**
**Geração de Search Queries:** Através de linguagem natural, é gerada pelo Gemini uma query para pesquisa técnica em sintaxe YARA-L 2.0.
**Geração de Regras:** Através de linguagem natural, é gerada pelo Gemini uma regra de detecção para ameaças.
**Threat Intelligence**: o Gemini pode responder perguntas e mostrar resumos sobre atores e indicadores.
**Security Playbooks**: O Gemini consegue criar e editar playbooks, para definição de workflows.

**Defesa e Segurança para aplicações com LLMs: Model Armor**
**Prompt Injection/Jailbreak Detection**: detecta e bloqueia inputs maliciosos.
**Sensitive Data Protection**: o Model Armor impede que dados sensíveis em prompts e em respostas.
**Content Safety:** O Model Armor possui filtros para conteúdo antiético, discurso de ódio, assédio e conteúdo perigoso.
**Malware**: o MA faz um scan em links em prompts e respostas para detectar conteúdo malicioso.
### Trust Principles
1. Os dados são seus, não da Google
2. A Google não vende os dados para terceiros
3. A confidencialidade é garantida, e os dados não são usados para direcionar anuncios
4. Todos os dados são criptografados por padrão
5. Funcionários da Google não tem acesso aos dados
6. Não existem backdoors para entidades ou governos
7. As políticas de privacidade são validadas e auditadas com padrões internacionais

### Residency & Compliance
A soberania de dados (Data Sovereignty) é o conceito legal de que os dados devem estar de acordo com as leis e regulações de onde eles residem. A GDPR na UE, por exemplo, exige que empresas obedecem leis de processamento e de proteção de dados de cidadãos. A residência de dados (Residency) trata do local onde, de fato, os dados esão armarmazenados. Algumas leis obrigam que os dados estejam fisicamente salvos em seu país em algumas situações.



#CDL #GCP 