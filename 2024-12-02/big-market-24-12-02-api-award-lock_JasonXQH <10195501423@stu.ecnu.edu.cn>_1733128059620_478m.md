### 代码评审

#### 1. MyBatis Mapper XML 文件变更

**文件：big-market-xqh-app/src/main/resources/mybatis/mapper/activity/IRaffleActivitySkuMapper.xml**

**变更点：**

- `updateSkuStock` 和 `clearActivitySkuStock` 方法参数类型从 `java.lang.Long` 更改为 `io.github.jasonxqh.infrastructure.dao.po.activity.RaffleActivitySku`。
- 在 `updateSkuStock` 和 `clearActivitySkuStock` 方法中添加了 `activity_id` 参数。
- 修改了 `updateSkuStock` 和 `clearActivitySkuStock` 方法的 SQL 语句，添加了 `activity_id` 条件。

**分析：**

- 修改参数类型为 `RaffleActivitySku` 更符合业务逻辑，因为库存信息通常与活动商品相关联。
- 添加 `activity_id` 参数和条件是合理的，因为库存和活动商品是关联的，应该根据活动ID进行更新和清理。
- SQL 语句的修改是必要的，以确保只针对特定的活动和商品更新和清理库存。

**建议：**

- 确保 `RaffleActivitySku` 类中有 `sku` 和 `activity_id` 属性。
- 检查其他使用 `IRaffleActivitySkuMapper` 的代码，确保它们也使用正确的参数类型和 SQL 语句。

#### 2. 测试代码变更

**文件：big-market-xqh-app/src/test/java/io/github/jasonxqh/domain/activity/service/RaffleOrderTest.java**

**变更点：**

- 在 `setUp` 方法中添加了 `rafleActivityAccountQuotaService` 的初始化。
- 修改了 `test_createSkuRechargeOrder` 方法的测试逻辑，增加了循环次数和异常处理。

**分析：**

- 添加 `rafleActivityAccountQuotaService` 的初始化是合理的，因为测试代码需要使用该服务。
- 增加循环次数和异常处理可以提高测试的覆盖率和可靠性。

**建议：**

- 确保 `rafleActivityAccountQuotaService` 的初始化代码正确无误。
- 进一步优化测试逻辑，例如添加更多的测试用例和断言。

#### 3. 其他代码变更

**文件：big-market-xqh-domain, big-market-xqh-infrastructure, big-market-xqh-trigger**

**变更点：**

- `IActivityRepository`, `IRaffleActivitySkuStockService`, `ActivityArmoryDispatch`, `IActivityDispatch`, `RaffleActivityAccountQuotaService` 等接口和类中添加了 `activity_id` 参数和相应的实现方法。
- `ActivityRepository`, `StrategyRepository`, `QueueManager` 等类中添加了 `activity_id` 相关的逻辑。
- `UpdateAwardStockJob`, `UpdateSkuStockJob`, `ActivitySkuStockZeroCustomer` 等类中添加了 `activity_id` 相关的逻辑。

**分析：**

- 这些变更是为了支持根据活动ID更新和清理库存。
- `activity_id` 的添加是合理的，因为库存和活动商品是关联的。

**建议：**

- 确保 `activity_id` 的使用符合业务逻辑和需求。
- 检查所有相关代码，确保它们正确处理 `activity_id`。

### 总结

这些代码变更是为了支持根据活动ID更新和清理库存。总体来说，这些变更是合理的，并且遵循了良好的编程实践。建议仔细检查所有相关代码，确保它们正确处理 `activity_id` 并符合业务逻辑和需求。