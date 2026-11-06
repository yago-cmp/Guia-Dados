###### Google **Cloud Storage** é um serviço de armazenamento em nuvem de objetos. O serviço é feito para guardar **dados não estruturados**. A mudança de "arquivos" para "objetos" se deve ao fato de que arquivos nos remetem à hierarquia, como pastas e diretórios. Os Objetos são guardados em binário, junto com metadados relevantes e um id global, a chave que o identifica, que é o nome do objeto. O tamano máximo de cada um é de 5TB. O acesso a eles é feito através de requisições HTTP. O Cloud Storage é um serviço altamente escalável e completamente gerenciado, servindo a diversos usos.
## Classes de Armazenamento
### Standard Storage
É o padrão para novos objetos, e é usado para itens que são **acessados com frequência**. O custo de acesso é o mais barato. Para dados de aplicação, é o mais indicado.
### Nearline Storage
Ideal para arquivos de backup, que não são acessados ou adicionados com uma grande frequência. O ideal, recomendado pela Google, é o acesso ser feito apenas uma vez por mês. O custo de acesso é um pouco mais caro, mas o preço por GB de armazenamento cai.
### Coldline Storage
Feito para dados raramente utilizados, frios. O acesso e alteração recomendado é de aproximadamente 90 dias, ou 3 meses. Um exemplo de uso são arquivos de Log, que vão se acumulando. Os custos de armazenamento caem mais, e os de acesso sobem.
### Archive Storage
Opção de custo de armazenamento mais baixo, para dados quase nunca acessados. O acesso a esses dados é o mais caro. O intervalo de acesso recomendado é de ~1 ano. Uma aplicação comum são dados de Compliance, que são atualizados anualmente.
### Autoclass
O Autoclass é uma feature que automaticamente direciona os objetos para uma classe de armazenamento julgada adequada, com base nas médias de frequência de acesso.
