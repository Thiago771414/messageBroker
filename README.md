# Message Brokers

Um Message Broker é uma tecnologia que atua como intermediário na comunicação entre diferentes componentes de um sistema distribuído. É como um sistema de correio que recebe, armazena e encaminha mensagens entre remetentes e destinatários.
Na implementação de Message Brokers, diferentes partes do sistema enviam e recebem mensagens por meio do Message Broker. O remetente envia uma mensagem para um tópico ou fila no Message Broker, e o destinatário pode se inscrever nesse tópico ou fila para receber e processar a mensagem.

Isso permite uma comunicação assíncrona e desacoplada entre os componentes do sistema, o que é especialmente útil em arquiteturas de microsserviços. O Message Broker garante que as mensagens sejam entregues corretamente, mesmo que os componentes estejam temporariamente indisponíveis, e pode fornecer recursos avançados, como roteamento de mensagens, filas de prioridade e entrega garantida.

exemplo de código em C# utilizando o RabbitMQ, um popular Message Broker, para ilustrar a comunicação assíncrona entre um produtor e um consumidor de mensagens:

```csharp
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System;
using System.Text;

class MessageBrokerExample
{
    static void Main(string[] args)
    {
        // Configuração do RabbitMQ
        var factory = new ConnectionFactory() { HostName = "localhost" };
        using (var connection = factory.CreateConnection())
        using (var channel = connection.CreateModel())
        {
            string queueName = "minha_fila";

            // Produtor de mensagens
            void SendMessage()
            {
                Console.WriteLine("Digite a mensagem a ser enviada:");
                string message = Console.ReadLine();

                channel.QueueDeclare(queue: queueName, durable: false, exclusive: false, autoDelete: false, arguments: null);

                var body = Encoding.UTF8.GetBytes(message);

                channel.BasicPublish(exchange: "", routingKey: queueName, basicProperties: null, body: body);
                Console.WriteLine("Mensagem enviada: {0}", message);
            }

            // Consumidor de mensagens
            void ConsumeMessages()
            {
                channel.QueueDeclare(queue: queueName, durable: false, exclusive: false, autoDelete: false, arguments: null);

                var consumer = new EventingBasicConsumer(channel);
                consumer.Received += (model, ea) =>
                {
                    var body = ea.Body.ToArray();
                    var message = Encoding.UTF8.GetString(body);
                    Console.WriteLine("Mensagem recebida: {0}", message);
                };

                channel.BasicConsume(queue: queueName, autoAck: true, consumer: consumer);

                Console.WriteLine("Aguardando mensagens. Pressione [Enter] para sair.");
                Console.ReadLine();
            }

            // Menu de seleção de ação
            bool exit = false;
            while (!exit)
            {
                Console.WriteLine("Selecione uma ação:");
                Console.WriteLine("1. Enviar mensagem");
                Console.WriteLine("2. Consumir mensagens");
                Console.WriteLine("3. Sair");

                string input = Console.ReadLine();
                switch (input)
                {
                    case "1":
                        SendMessage();
                        break;
                    case "2":
                        ConsumeMessages();
                        break;
                    case "3":
                        exit = true;
                        break;
                    default:
                        Console.WriteLine("Opção inválida. Tente novamente.");
                        break;
                }
            }
        }
    }
}
```
O uso de um Message Broker, como o RabbitMQ, oferece uma maneira robusta e escalável de implementar a comunicação assíncrona e desacoplada entre componentes de sistemas distribuídos. Ele garante a entrega correta das mensagens, mesmo que os componentes estejam temporariamente indisponíveis, e oferece recursos avançados, como roteamento de mensagens, filas de prioridade e entrega garantida.

## Livro
![Imagem](https://m.media-amazon.com/images/I/61YQ8Tz8sUL._SY342_.jpg)
