# Tableau 三个 Dashboard 最终搭建指南：Cluster Map + Residual Map

这版结构适合你现在想做的 3-dashboard submission。核心原则是：**不是图越多越好，而是每一页回答一个明确问题**。

## 最终结构

### Dashboard 1 - Spatial outcome
**问题：poor health 在哪里集中？2011-2021 有什么变化？**

保留你现在的两张地图：

- `Map - Poor health 2021`
- `Map - Change in poor health`

右侧只保留两个色带图例：

- `Poor Health 2021 Pct`
- `Poor Health Change 2021 Minus 2011 Pctpt`

底部 annotation：

```text
Poor health remains spatially structured across local authorities, with higher values concentrated in parts of the North, coastal areas and some urban-industrial regions. The change map suggests that these inequalities persisted between 2011 and 2021, although the change view should be interpreted cautiously because the 2011 and 2021 health measures use different standardisation.
```

---

## Dashboard 2 - Structural profiles

**问题：PCA/Isomap 得到的 cluster 在地理上分布在哪里？**

推荐布局：

```text
[ Cluster profile map ]   [ PCA projection       ]
[ Cluster profile map ]   [ Isomap projection    ]
[ short annotation across bottom                 ]
```

也就是：左边一张大 cluster map，右边上下放 PCA 和 Isomap。

### 新增图 1：Cluster Profile Map

1. 新建 worksheet。
2. 左上角数据源选择：

```text
tableau_map_hybrid_unknown_fix
```

3. 右键字段：

```text
Map Authority Code Hybrid
```

选择：

```text
地理角色 -> UKLocalAuthorityDistricts -> Code
```

4. 双击：

```text
Map Authority Code Hybrid
```

5. 如果是点地图，选择：

```text
智能显示 -> 填充地图
```

或在「标记」下拉框选择：

```text
地图
```

6. 把下面字段拖到颜色：

```text
Cluster Profile
```

7. 工具提示建议放：

```text
Map Current Authority Name
Cluster Profile
Poor Health 2021 Pct
Social Rented 2021 Pct
Private Rented 2021 Pct
Overcrowded Bedrooms 2021 Pct
Econ Inactive 2021 Pct
Map Boundary Note
```

8. 标题建议：

```text
Geography of multivariate inequality profiles
```

9. 颜色要和 PCA / Isomap 里的 `Cluster Profile` 保持一致。因为 cluster map 使用的是另一个数据源，Tableau 可能自动分配不同颜色。需要手动检查：

```text
颜色 -> 编辑颜色
```

让同名 cluster 在 Cluster Map、PCA、Isomap 里颜色一致。

### Dashboard 2 annotation

放在底部：

```text
Cluster profiles connect the projection views back to geography. PCA and Isomap show similarity between local authorities, while the cluster map shows where those multivariate profiles are located.
```

---

## Dashboard 3 - Bayesian model diagnostics

**问题：模型哪里解释得好，哪里解释不了？**

推荐布局：

```text
[ Bayesian actual vs predicted - wide top        ]
[ Bayesian residual map        ] [ Profile chart ]
[ short annotation across bottom                 ]
```

如果空间不够，也可以：

```text
[ Bayesian actual vs predicted ] [ Bayesian residual map ]
[ Selected or average profile chart - full width ]
```

### 新增图 2：Bayesian Residual Map

1. 新建 worksheet。
2. 左上角数据源选择：

```text
tableau_map_hybrid_unknown_fix
```

3. 右键字段：

```text
Map Authority Code Hybrid
```

选择：

```text
地理角色 -> UKLocalAuthorityDistricts -> Code
```

4. 双击 `Map Authority Code Hybrid` 生成地图。
5. 把下面字段拖到颜色：

```text
Bayes Residual Actual Minus Predicted Pctpt
```

字段在 CSV 里的原名是：

```text
bayes_residual_actual_minus_predicted_pctpt
```

6. 颜色设置：

```text
颜色 -> 编辑颜色
```

推荐：

- 使用发散色带；
- 中心设为 `0`；
- 负数 = actual lower than predicted；
- 正数 = actual higher than predicted。

如果你的色带是蓝-橙，建议：

```text
负数 / lower than predicted = 蓝色
正数 / higher than predicted = 橙色或红色
```

7. 工具提示建议放：

```text
Map Current Authority Name
Poor Health 2021 Pct
Bayes Poor Health 2021 Predicted Pct
Bayes Residual Actual Minus Predicted Pctpt
Bayes Poor Health 2021 Ci Low Pct
Bayes Poor Health 2021 Ci High Pct
Cluster Profile
Map Boundary Note
```

8. 标题建议：

```text
Bayesian residual map: observed minus predicted poor health
```

### Dashboard 3 annotation

放在底部：

```text
Residuals show where the Bayesian model over- or under-predicts poor health. Positive residuals mean observed poor health is higher than predicted; negative residuals mean observed poor health is lower than predicted. This view supports model evaluation rather than causal explanation.
```

---

## Dashboard actions 建议

### Dashboard 2

菜单：

```text
仪表板 -> 操作...
```

添加筛选动作：

```text
名称：Update structural profile views
源工作表：Cluster profile map, PCA, Isomap
运行操作方式：选择
目标工作表：PCA, Isomap, selected/average profile chart
清除选择时：显示所有值
```

如果筛选动作让图变空或太跳，可以改成「突出显示」动作。

### Dashboard 3

添加筛选动作：

```text
名称：Update model diagnostic profile
源工作表：Bayesian actual vs predicted, Bayesian residual map
运行操作方式：选择
目标工作表：Selected or average profile chart
清除选择时：显示所有值
```

---

## 重要注意事项

1. `Cluster Map` 和 `Residual Map` 使用 `tableau_map_hybrid_unknown_fix.csv`。
2. PCA、Isomap、Bayesian actual vs predicted 仍然用 `tableau_master.csv`。
3. 不要用 `数据 -> 替换数据源` 一键替换全 workbook，否则可能把散点图也改成 348 行的 map source。
4. `Profile chart` 里的 percentage-point fields 必须用 `平均值 / AVG`，不要用 `总和 / SUM`。
5. 最终保存必须用：

```text
文件 -> 另存为 -> Tableau Packaged Workbook (*.twbx)
```

---

## 最终三页标题建议

```text
Dashboard 1: Health inequality across England and Wales
Dashboard 2: Structural profiles of health, housing and economic inequality
Dashboard 3: Bayesian model diagnostics and residual geography
```
