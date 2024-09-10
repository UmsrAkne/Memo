# Moq 

Moq の使用例を生成 AI に書いてもらった。※未検証

NUnit と Moq を使用したテストの基本的な例を紹介します。ここでは、サービスクラス OrderService をテスト対象とし、依存するリポジトリ IOrderRepository をモックします。

### テスト対象のコード例

    public interface IOrderRepository
    {
        void SaveOrder(Order order);
        Order GetOrder(int orderId);
    }

    public class Order
    {
        public int Id { get; set; }
        public string ProductName { get; set; }
        public int Quantity { get; set; }
    }

    public class OrderService
    {
        private readonly IOrderRepository _orderRepository;

        public OrderService(IOrderRepository orderRepository)
        {
            _orderRepository = orderRepository;
        }

        public void ProcessOrder(Order order)
        {
            if (order.Quantity <= 0)
            {
                throw new ArgumentException("Quantity must be greater than zero");
            }
            _orderRepository.SaveOrder(order);
        }

        public Order GetOrder(int orderId)
        {
            return _orderRepository.GetOrder(orderId);
        }
    }

### テストクラスのコード例

    using Moq;
    using NUnit.Framework;

    [TestFixture]
    public class OrderServiceTests
    {
        private Mock<IOrderRepository> _mockOrderRepository;
        private OrderService _orderService;

        [SetUp]
        public void SetUp()
        {
            // Moq を使用して IOrderRepository のモックを作成
            _mockOrderRepository = new Mock<IOrderRepository>();

            // モックを OrderService に渡す
            _orderService = new OrderService(_mockOrderRepository.Object);
        }

        [Test]
        public void ProcessOrder_ValidOrder_CallsSaveOrder()
        {
            // Arrange: テスト用の Order インスタンスを作成
            var order = new Order { Id = 1, ProductName = "Laptop", Quantity = 2 };

            // Act: ProcessOrder メソッドを呼び出す
            _orderService.ProcessOrder(order);

            // Assert: SaveOrder メソッドが一度だけ呼び出されたか確認
            _mockOrderRepository.Verify(r => r.SaveOrder(order), Times.Once);
        }

        [Test]
        public void ProcessOrder_InvalidOrder_ThrowsArgumentException()
        {
            // Arrange: 無効な注文（Quantity が 0）の作成
            var order = new Order { Id = 2, ProductName = "Phone", Quantity = 0 };

            // Act & Assert: 例外がスローされることを確認
            Assert.Throws<ArgumentException>(() => _orderService.ProcessOrder(order));
        }

        [Test]
        public void GetOrder_ExistingOrder_ReturnsOrder()
        {
            // Arrange: モックが GetOrder メソッドで特定の注文を返すように設定
            var order = new Order { Id = 3, ProductName = "Tablet", Quantity = 1 };
            _mockOrderRepository.Setup(r => r.GetOrder(3)).Returns(order);

            // Act: GetOrder メソッドを呼び出す
            var result = _orderService.GetOrder(3);

            // Assert: GetOrder が正しい Order を返すか確認
            Assert.AreEqual(order, result);
        }
    }

ポイント
- Moq を使用して IOrderRepository のモックを作成し、テスト対象クラスに注入します。
- Verify メソッドを使って、特定のメソッドが期待通りに呼び出されたか確認します。
- Setup メソッドを使用して、モックが特定のメソッド呼び出しに対して返す値を指定します。
- NUnit の [Test] 属性を使用して、個別のテストケースを定義します。
- この例では、ProcessOrder メソッドと GetOrder メソッドの動作をテストしています。

注意: 本文は、AIであるChatGPTによって生成されています。内容の正確性や最新性については、ご自身での確認をお願いします。