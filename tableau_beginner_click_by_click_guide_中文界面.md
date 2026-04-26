# Tableau 中文界面逐步搭建指南

本指南专门对应中文界面的 **Tableau Desktop**，使用文件：

```text
VISA_final_submission_pack/data/clean/tableau_master.csv
```

目标是做出可以提交的 Tableau packaged workbook：

```text
VISA_Coursework_Health_Housing_Economic_Inequality.twbx
```

本作业主要用 **Tableau Desktop**，不需要使用 Tableau Prep。

---

## 0. 先准备英国地图识别

如果你已经能在 `地理角色` 里看到 `UKLocalAuthorityDistricts`，这一节可以跳过。

1. 完全关闭 Tableau Desktop。
2. 解压 `Local Data (4).zip`。
3. 找到里面真正的 `Local Data` 文件夹。这个文件夹里应该直接包含这些文件：

```text
GEOCODING.FDB
UKLocalAuthorityDistricts.tds
UKLowerSuperOutputAreas.tds
GBMiddleSuperOutputAreas.tds
GBPostcodeDistricts.tds
GBPostcodeSectors.tds
...
```

4. 因为你是中文界面，如果 Tableau 识别不到，可以把文件夹名从：

```text
Local Data
```

改成：

```text
本地数据
```

5. 把这个文件夹复制到 Tableau 存储库。Windows 常见位置是下面两个之一：

```text
C:\Users\你的用户名\Documents\My Tableau Repository\本地数据\
```

或者：

```text
C:\Users\你的用户名\文档\我的 Tableau 存储库\本地数据\
```

6. 最终路径应该像这样：

```text
...\我的 Tableau 存储库\本地数据\GEOCODING.FDB
```

不要变成这种多套一层的路径：

```text
...\我的 Tableau 存储库\本地数据\Local Data\GEOCODING.FDB
```

7. 重新打开 Tableau Desktop。

---

## 1. 连接主数据

1. 打开 **Tableau Desktop**。
2. 在左侧 **连接** 面板中，点击：

```text
到文件 → 文本文件
```

3. 选择这个文件：

```text
VISA_final_submission_pack/data/clean/tableau_master.csv
```

4. 打开后会进入 **数据源** 页面。底部点击：

```text
工作表 1
```

进入正式画图界面。

---

## 2. 先设置地理角色

进入工作表后，在左侧字段列表里找到：

```text
Ltla Code
Ltla Name
```

Tableau 会把 CSV 里的 `ltla_code` 显示成 `Ltla Code`，把 `ltla_name` 显示成 `Ltla Name`，这是正常的。

### 推荐设置：用代码画地图

右键左侧字段列表里的：

```text
Ltla Code
```

选择：

```text
地理角色 → UKLocalAuthorityDistricts → Code
```

设置成功后，`Ltla Code` 前面的图标应该会变成小地球。

### 辅助设置：给地区名也设置地理角色

右键：

```text
Ltla Name
```

选择：

```text
地理角色 → UKLocalAuthorityDistricts → Name
```

之后画地图时，优先双击 `Ltla Code`，因为代码比地名更稳定。`Ltla Name` 主要放在标签和工具提示里显示地名。

---

## 3. 字段类型检查

这一步不用每个字段都手动改，只要发现字段位置不对时再改。

### 3.1 应该是维度的字段

这些字段应该在左侧上半部分的 **维度** 区域：

```text
Ltla Code
Ltla Name
Country
Cluster
Cluster Profile
Source 2011 Authorities
```

如果 `Cluster` 跑到了度量区，操作：

```text
右键 Cluster → 转换为维度
```

`Cluster` 虽然看起来是数字，但它是类别编号，不是连续数值。拖到颜色时应该是蓝色离散字段。

### 3.2 应该是度量的字段

这些字段应该在左侧下半部分的 **度量** 区域：

```text
Poor Health 2021 Pct
Poor Health 2011 Pct
Poor Health Change 2021 Minus 2011 Pctpt
Social Rented 2021 Pct
Private Rented 2021 Pct
Overcrowded Bedrooms 2021 Pct
Econ Inactive 2021 Pct
Inactive Long Term Sick Disabled 2021 Pct
Pca 1
Pca 2
Isomap 1
Isomap 2
Bayes Poor Health 2021 Predicted Pct
Bayes Residual Actual Minus Predicted Pctpt
Source 2011 Authority Count
```

`Source 2011 Authority Count` 是数字字段，放在度量里是对的。它主要用于说明一个 2021 地区由几个 2011 地区合并而来。

### 3.3 连续 / 离散检查

做图时，如果某个数值字段被拖到行、列、颜色后变成蓝色，但它应该是连续数值，就右键这个字段胶囊，选择：

```text
连续
```

这些字段通常应该是绿色连续字段：

```text
所有 Pct 字段
所有 Pctpt 字段
Pca 1
Pca 2
Isomap 1
Isomap 2
Bayes 相关数值字段
```

`Cluster` 和 `Cluster Profile` 应该是蓝色离散字段。

---

## 4. 单位格式提醒

CSV 里的百分比字段已经是百分数值，例如 `6.5` 表示 `6.5%`，不是 `0.065`。

所以不要把它们格式化成 Tableau 的“百分比”格式，否则可能显示成 `650%`。

推荐做法：

```text
右键字段 → 默认属性 → 数字格式 → 数字（自定义）
```

然后设置：

```text
小数位数：1
后缀：%
```

对于 `Pctpt` 字段，可以设置后缀：

```text
ppt
```

不过这一步不是必须。只要图标题和工具提示里写清楚单位，也可以。

---

## 5. 工作表 A：2021 Poor Health 地图

1. 点击底部新建工作表按钮，或使用当前 `工作表 1`。
2. 将工作表重命名为：

```text
Map - Poor health 2021
```

重命名方法：右键底部工作表标签 → **重命名**。

3. 双击左侧字段：

```text
Ltla Code
```

Tableau 应该生成英国地图。

4. 在 **标记** 卡里，把标记类型从 `自动` 改成：

```text
地图
```

如果地图不是填充区域，可以点击右侧：

```text
智能显示
```

选择 **填充地图**。

5. 把下面字段拖到 **颜色**：

```text
Poor Health 2021 Pct
```

6. 把下面字段拖到 **详细信息**：

```text
Ltla Name
```

7. 把下面字段拖到 **工具提示**：

```text
Ltla Name
Poor Health 2021 Pct
Poor Health 2011 Pct
Poor Health Change 2021 Minus 2011 Pctpt
Social Rented 2021 Pct
Overcrowded Bedrooms 2021 Pct
Econ Inactive 2021 Pct
Cluster Profile
Source 2011 Authorities
```

8. 修改标题：双击图上方标题，改成：

```text
2021 poor health, age-standardised (%)
```

9. 如果颜色太难看，点击颜色图例右上角小三角：

```text
编辑颜色
```

选择一个连续色带即可。

---

## 6. 工作表 B：2011–2021 Poor Health 变化地图

1. 在底部右键 `Map - Poor health 2021` 工作表标签。
2. 选择：

```text
复制
```

3. 把复制出来的新工作表重命名为：

```text
Map - Change in poor health
```

4. 在 **标记** 卡的 **颜色** 上，移除原来的：

```text
Poor Health 2021 Pct
```

5. 把下面字段拖到 **颜色**：

```text
Poor Health Change 2021 Minus 2011 Pctpt
```

6. 点击颜色图例的小三角：

```text
编辑颜色
```

选择一个发散色带，让负值和正值容易区分。

7. 工具提示里保留或加入：

```text
Ltla Name
Poor Health 2011 Pct
Poor Health 2021 Pct
Poor Health Change 2021 Minus 2011 Pctpt
Source 2011 Authorities
Source 2011 Authority Count
```

8. 修改标题为：

```text
Change in poor health, 2011–2021 percentage points
```

9. 在标题或说明里记住这个解释：

```text
2011 uses raw percentage; 2021 uses age-standardised TS037ASP percentage, so this change view is exploratory.
```

这句话之后可以写进报告的 limitation。

---

## 7. 工作表 C：PCA Projection 散点图

1. 新建工作表，命名为：

```text
Projection - PCA
```

2. 把下面字段拖到 **列**：

```text
Pca 1
```

3. 把下面字段拖到 **行**：

```text
Pca 2
```

4. 确保 `Pca 1` 和 `Pca 2` 是绿色连续字段。如果是蓝色，右键字段胶囊 → **连续**。

5. 把下面字段拖到 **详细信息**：

```text
Ltla Name
```

6. 把下面字段拖到 **颜色**：

```text
Cluster Profile
```

7. 把下面字段拖到 **大小**：

```text
Poor Health 2021 Pct
```

8. 把下面字段拖到 **工具提示**：

```text
Ltla Name
Country
Cluster Profile
Poor Health 2021 Pct
Social Rented 2021 Pct
Private Rented 2021 Pct
Overcrowded Bedrooms 2021 Pct
Econ Inactive 2021 Pct
Inactive Long Term Sick Disabled 2021 Pct
```

9. 标题改成：

```text
PCA projection of health, housing and economic indicators
```

10. 如果点太大或太小，点击 **大小** 调整。

---

## 8. 工作表 D：Isomap Projection 散点图

1. 右键底部 `Projection - PCA` 标签。
2. 选择：

```text
复制
```

3. 把新工作表命名为：

```text
Projection - Isomap
```

4. 把 **列** 上的 `Pca 1` 替换成：

```text
Isomap 1
```

5. 把 **行** 上的 `Pca 2` 替换成：

```text
Isomap 2
```

6. 其他设置保持一样：

```text
Cluster Profile → 颜色
Poor Health 2021 Pct → 大小
Ltla Name → 详细信息 / 工具提示
```

7. 标题改成：

```text
Isomap projection of non-linear local authority similarity
```

---

## 9. 工作表 E：Bayesian actual vs predicted

1. 新建工作表，命名为：

```text
Bayesian - actual vs predicted
```

2. 把下面字段拖到 **列**：

```text
Poor Health 2021 Pct
```

3. 把下面字段拖到 **行**：

```text
Bayes Poor Health 2021 Predicted Pct
```

4. 把下面字段拖到 **详细信息**：

```text
Ltla Name
```

5. 把下面字段拖到 **颜色**：

```text
Cluster Profile
```

6. 把下面字段拖到 **工具提示**：

```text
Ltla Name
Poor Health 2021 Pct
Bayes Poor Health 2021 Predicted Pct
Bayes Poor Health 2021 Ci Low Pct
Bayes Poor Health 2021 Ci High Pct
Bayes Residual Actual Minus Predicted Pctpt
Cluster Profile
```

7. 打开左侧上方的 **分析** 面板。
8. 拖一个：

```text
趋势线
```

到图中，或者添加参考线帮助判断拟合。

9. 标题改成：

```text
Bayesian model: actual vs predicted poor health
```

---

## 10. 工作表 F：Selected area profile / change bars

这一张图用于 dashboard 底部，展示一个地区的几个关键变化指标。这里不需要第二个数据源，只用 `tableau_master.csv`。

1. 新建工作表，命名为：

```text
Profile - change indicators
```

2. 在左侧字段列表里找到 Tableau 自带字段：

```text
度量名称
度量值
```

英文数据字段较多时，它们通常在左侧底部附近。

3. 把 **度量名称** 拖到 **行**。
4. 把 **度量值** 拖到 **列**。
5. 把 **度量名称** 再拖到 **筛选器**。
6. 在弹出的窗口里，只勾选这些字段：

```text
Poor Health Change 2021 Minus 2011 Pctpt
Social Rented Change 2021 Minus 2011 Pctpt
Private Rented Change 2021 Minus 2011 Pctpt
Overcrowded Bedrooms Change 2021 Minus 2011 Pctpt
Econ Inactive Change 2021 Minus 2011 Pctpt
Inactive Long Term Sick Disabled Change 2021 Minus 2011 Pctpt
```

7. 标记类型选择：

```text
条形图
```

8. 把下面字段拖到 **工具提示**：

```text
Ltla Name
```

9. 标题改成：

```text
Selected local authority: change indicators, 2011–2021
```

10. 这张图在 dashboard 里会通过点击地图或散点图来筛选。如果没有选择任何地区，它可能显示所有地区的聚合结果；这是正常的。最终 dashboard 里用筛选操作控制它。

---

## 11. 可选工作表 G：Top poor health bar chart

这张图不是必须，但可以让 dashboard 更容易解释。

1. 新建工作表，命名为：

```text
Top - poor health 2021
```

2. 把 `Ltla Name` 拖到 **行**。
3. 把 `Poor Health 2021 Pct` 拖到 **列**。
4. 点击工具栏上的排序按钮，按 poor health 从高到低排序。
5. 右键 `Ltla Name` → **筛选器** → 选择 **顶部**。
6. 设置：

```text
按字段 → 前 10 个 → Poor Health 2021 Pct → 平均值 或 总计
```

由于每个地区只有一行，平均值和总计效果基本一样。

7. 标题改成：

```text
Top 10 local authorities by poor health in 2021
```

---

## 12. 建立主 Dashboard

1. 底部点击：

```text
新建仪表板
```

2. 把仪表板命名为：

```text
Health, Housing and Economic Inequality 2011–2021
```

3. 左侧 **大小** 建议选：

```text
自动
```

或者选择一个固定大小，例如：

```text
宽 1400，高 900
```

4. 推荐布局：

```text
左侧大图：Map - Poor health 2021
右上：Projection - PCA
右中：Projection - Isomap
右下：Bayesian - actual vs predicted
底部：Profile - change indicators
```

如果空间不够，可以把 `Bayesian - actual vs predicted` 放到第二个 dashboard。

5. 添加筛选器：

在 dashboard 里点击地图右上角的小三角，选择：

```text
筛选器 → Country
```

再添加：

```text
筛选器 → Cluster Profile
```

6. 筛选器显示出来后，右键筛选器标题，可以选择：

```text
单值下拉列表
```

或者：

```text
多值下拉列表
```

---

## 13. 设置 Dashboard 交互

这是拿高分很重要的一步：点击地图或散点图时，其他图也要跟着变化。

### 13.1 地图筛选其他图

1. 在顶部菜单点击：

```text
仪表板 → 操作...
```

2. 点击：

```text
添加操作 → 筛选器...
```

3. 名称写：

```text
Map filters dashboard
```

4. 源工作表选择：

```text
Map - Poor health 2021
```

5. 目标工作表选择：

```text
Projection - PCA
Projection - Isomap
Bayesian - actual vs predicted
Profile - change indicators
Top - poor health 2021（如果你做了这张）
```

6. 运行操作方式选择：

```text
选择
```

7. 清除选择将会选择：

```text
显示所有值
```

8. 点击确定。

### 13.2 PCA 筛选其他图

重复上面的步骤，再添加一个筛选操作：

```text
源工作表：Projection - PCA
目标工作表：Map - Poor health 2021、Projection - Isomap、Bayesian - actual vs predicted、Profile - change indicators
运行操作方式：选择
清除选择将会：显示所有值
```

### 13.3 检查交互是否成功

在 dashboard 里点击一个地区或一个散点。成功时应该看到：

```text
地图选中一个地区
PCA / Isomap 图只显示或突出对应地区
Bayesian 图跟着筛选
底部 profile bar chart 显示该地区的变化指标
```

---

## 14. 工具提示建议文字

工具提示不要只显示字段名，最好改得像解释性文字。

操作：点击 **标记** 卡里的：

```text
工具提示
```

可以改成类似下面这样：

```text
<ATTR(Ltla Name)>
Country: <ATTR(Country)>
Cluster: <ATTR(Cluster Profile)>

Poor health 2021: <SUM(Poor Health 2021 Pct)>%
Poor health 2011: <SUM(Poor Health 2011 Pct)>%
Change: <SUM(Poor Health Change 2021 Minus 2011 Pctpt)> ppt

Social rented 2021: <SUM(Social Rented 2021 Pct)>%
Overcrowded bedrooms 2021: <SUM(Overcrowded Bedrooms 2021 Pct)>%
Economic inactivity 2021: <SUM(Econ Inactive 2021 Pct)>%

2011 source authority mapping:
<ATTR(Source 2011 Authorities)>
```

字段插入时不用手打尖括号。可以在工具提示编辑窗口里点击 **插入**，选择对应字段。

---

## 15. 保存为 `.twbx`

最终一定要保存成 packaged workbook。

1. 顶部菜单点击：

```text
文件 → 另存为...
```

2. 文件类型选择：

```text
Tableau Packaged Workbook (*.twbx)
```

3. 文件名建议：

```text
VISA_Coursework_Health_Housing_Economic_Inequality.twbx
```

4. 保存后，不要只交 `.twb`。必须交 `.twbx`，因为 `.twbx` 会把 workbook 和本地 CSV 数据打包在一起。

---

## 16. 提交前检查清单

提交前逐项确认：

```text
□ 使用的是 Tableau Desktop，不是 Tableau Prep。
□ 数据源是 tableau_master.csv。
□ Ltla Code 已设置为 UKLocalAuthorityDistricts → Code。
□ 至少有一张地图。
□ 至少有 PCA projection。
□ 至少有 Isomap projection。
□ 有 Bayesian actual vs predicted 或 Bayesian residual 相关图。
□ 有 2011–2021 change 图。
□ Dashboard 有交互：点击地图或 PCA 后，其他图会变化。
□ 每张图都有清楚标题。
□ Tooltip 里有地区名、关键数值和单位。
□ 最终保存为 .twbx，不是 .twb。
□ 报告里的 student name / ID placeholder 已经替换。
□ 报告里的 peer evaluation placeholder 已经替换成真实反馈。
```

---

## 17. 常见问题

### 问题 1：找不到 UKLocalAuthorityDistricts

原因通常是 `本地数据` 文件夹路径不对。检查最终路径是不是：

```text
...\我的 Tableau 存储库\本地数据\GEOCODING.FDB
```

如果中间多套了一层文件夹，Tableau 会读不到。

### 问题 2：地图显示很多未知位置

优先使用：

```text
Ltla Code → 地理角色 → UKLocalAuthorityDistricts → Code
```

不要只用 `Ltla Name → Name`。

### 问题 3：百分比显示成 600% 或 900%

这是因为 Tableau 把 `6.0` 当成 600% 了。不要用百分比格式。改用：

```text
数字（自定义）→ 后缀 %
```

### 问题 4：Cluster 颜色是连续渐变

`Cluster` 应该是类别，不是连续数值。操作：

```text
右键 Cluster → 转换为维度
```

或者在颜色上的 `Cluster` 胶囊上右键：

```text
离散
```

更推荐直接用：

```text
Cluster Profile → 颜色
```

### 问题 5：PCA / Isomap 图不像散点图

检查：

```text
Pca 1、Pca 2、Isomap 1、Isomap 2 是否是绿色连续字段
Ltla Name 是否放到了详细信息
Cluster Profile 是否放到了颜色
```

---

## 18. 建议最终 dashboard 标题

可以用这个标题：

```text
Health, Housing and Economic Inequality in England and Wales, 2011–2021
```

副标题可以写：

```text
Local authority profiles combining Census health, housing and economic activity indicators; PCA, Isomap and Bayesian model views.
```
