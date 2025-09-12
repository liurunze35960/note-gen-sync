## 1. 什么是 Voronoi 图？

想象一下，你在一个平面上撒下了一些点（我们称它们为“种子点”或“站点”）。对于平面上的任何一个位置，我们想知道它离哪个种子点最近。Voronoi 图就是这样一种划分：它将平面划分为多个区域，每个区域都与一个种子点相关联，并且该区域内的所有点都比其他任何种子点更接近这个关联的种子点。

**简单来说：** Voronoi 图是根据一组离散点将空间划分为多个区域的图形，每个区域包含且仅包含离其对应点最近的所有位置。

这些区域被称为 **Voronoi 单元** (Voronoi cells) 或 **泰森多边形** (Thiessen polygons)。Voronoi 单元的边界是线段，这些线段是相邻两个种子点之间垂直平分线的一部分。Voronoi 图中的顶点是三个或更多 Voronoi 单元相交的点。


## 2. 核心概念与定义

* **种子点 (Sites/Generators):** 平面上一组预先给定的离散点，它们是 Voronoi 图构建的基础。
* **Voronoi 单元 (Voronoi Cell):** 与某个种子点 $P_i$ 相关联的区域 $R_i$，该区域内的任何点 $x$ 到 $P_i$ 的距离都小于或等于它到任何其他种子点 $P_j$ (其中 $j \neq i$) 的距离。
    * 数学表示：$R_i = \{ x \in \text{平面} \mid d(x, P_i) \le d(x, P_j) \text{ for all } j \neq i \}$
    * 其中 $d(x, P)$ 表示点 $x$ 和点 $P$ 之间的欧几里得距离。
* **Voronoi 边 (Voronoi Edge):** 相邻两个 Voronoi 单元的公共边界。这条边上的任意一点到这两个单元对应的种子点的距离相等。它实际上是这两个种子点连线的垂直平分线的一部分。
* **Voronoi 顶点 (Voronoi Vertex):** 三个或更多 Voronoi 单元的交点。这个点到这三个（或更多）单元对应的种子点的距离相等。因此，它也是这三个（或更多）种子点外接圆的圆心。

## 3. Voronoi 图的主要性质

* **凸多边形:** 每个 Voronoi 单元都是一个凸多边形。这意味着单元内任意两点的连线段完全包含在单元内。
* **唯一性:** 对于给定的一组不共线的种子点，其 Voronoi 图是唯一的。
* **邻近关系:** Voronoi 图清晰地展示了种子点之间的邻近关系。如果两个种子点的 Voronoi 单元共享一条边，则它们互为 Voronoi 邻居。
* **空圆特性:** 对于 Voronoi 图中的任意一个 Voronoi 顶点，以该顶点为圆心，以其到对应种子点的距离为半径画一个圆，这个圆内不会包含任何其他的种子点。
* **边界:** 如果所有种子点都在一个凸包内，那么最外层的 Voronoi 单元是无界的（延伸到无穷远）。
* **边的数量:** 如果有 $n$ 个种子点，Voronoi 图最多有 $2n-5$ 个顶点和 $3n-6$ 条边 (对于 $n \ge 3$)。

## 4. 如何构建 Voronoi 图？(概念性理解)

构建 Voronoi 图的算法有很多，对于初学者来说，理解其基本思想比掌握复杂算法更重要。

**基本思想：**

1.  **垂直平分线：** 对于任意两个种子点 $P_i$ 和 $P_j$，它们连线的垂直平分线将平面分为两部分。一部分离 $P_i$ 更近，另一部分离 $P_j$ 更近。这条垂直平分线就是构成 Voronoi 单元边界的基础。
2.  **区域的交集：** 一个种子点 $P_i$ 的 Voronoi 单元，实际上是所有以 $P_i$ 为“更近”一方的半平面的交集。

**常见算法 (了解即可)：**

* **暴力算法 (Brute Force):** 对于平面上的每个像素点（或足够密的采样点），计算它到所有种子点的距离，然后将其归类到最近的种子点。这种方法简单但效率低下。
* **增量构造法 (Incremental Construction):** 逐个加入种子点，并更新已有的 Voronoi 图。
* **分治算法 (Divide and Conquer):** 将种子点集分成两半，分别计算它们的 Voronoi 图，然后合并结果。
* **Fortune 算法 (Sweep Line Algorithm):** 一种高效的平面扫描算法，是实际应用中最常用的算法之一。它通过一条扫描线扫过平面，动态地构建 Voronoi 图的边。
* **Bowyer-Watson 算法:** 常用于生成 Delaunay 三角剖分 (Delaunay Triangulation)，而 Delaunay 三角剖分与 Voronoi 图是对偶关系，可以相互转换。

对于初学者，可以尝试手动绘制一个简单的 Voronoi 图：
1.  在纸上画出 3-4 个种子点。
2.  对于每对种子点，画出它们连线的垂直平分线。
3.  找出这些垂直平分线如何划分平面，形成每个种子点的最近邻区域。

## 5. Voronoi 图的应用领域

Voronoi 图由于其独特的空间划分特性，在众多领域都有广泛应用：

* **最近邻搜索 (Nearest Neighbor Search):**
    * 快速找到离查询点最近的种子点。例如，找到离你当前位置最近的医院、餐馆等。
* **计算几何 (Computational Geometry):**
    * 作为许多其他几何算法的基础。
* **地理信息系统 (GIS):**
    * 服务区划分（如学校、消防局的覆盖范围）。
    * 降雨量分布（每个气象站控制的区域）。
* **计算机图形学 (Computer Graphics):**
    * 生成不规则纹理、细胞结构、破碎效果。
    * 路径规划。
* **数据分析与模式识别:**
    * 聚类分析，将数据点划分到最近的簇中心。
* **生物学:**
    * 模拟细胞组织结构、生态系统中物种的势力范围。
* **机器人学:**
    * 路径规划，传感器覆盖范围分析。
* **材料科学:**
    * 分析晶体结构、微观结构。
* **网络规划:**
    * 无线网络基站的覆盖范围优化。

## 6. Voronoi 图的优缺点

**优点:**

* **直观性:** 清晰地展示了空间中点的邻近关系。
* **唯一性:** 对于给定的点集，结果是确定的。
* **高效性:** 有成熟的高效构建算法。
* **应用广泛:** 解决各种与“最近”相关的问题。

**缺点:**

* **对种子点敏感:** 种子点的微小变动可能导致 Voronoi 图的较大变化。
* **仅基于距离:** 默认使用欧几里得距离，可能不适用于所有场景（例如，考虑城市街道距离时，欧氏距离就不太准确，需要使用曼哈顿距离等）。
* **高维计算复杂:** 在三维或更高维度空间中，Voronoi 图的计算和可视化变得非常复杂。

## 7. Delaunay 三角剖分 (Delaunay Triangulation) - 重要的伙伴

Delaunay 三角剖分与 Voronoi 图是**对偶图 (Dual Graph)**。

* 如果将 Voronoi 图中共享一条边的两个种子点连接起来，就会得到一个 Delaunay 三角剖分。
* 反之，Delaunay 三角形的外接圆圆心就是 Voronoi 图的顶点。

**Delaunay 三角剖分的特性：**

* **空圆特性:** Delaunay 三角剖分中的任何一个三角形，其外接圆内部不包含点集中的任何其他点。
* **最大化最小角:** 在所有可能的三角剖分中，Delaunay 三角剖分倾向于避免出现过于狭长的三角形，使得三角形的最小角尽可能大。

理解 Delaunay 三角剖分有助于更深入地理解 Voronoi 图。

## 8. 学习资源推荐

* **可视化工具:**
    * 在网上搜索 "Voronoi diagram generator" 或 "interactive Voronoi diagram"，有很多在线工具可以让你交互式地放置点并观察生成的 Voronoi 图。例如：
        * [https://alexbeutel.com/webgl/voronoi.html](https://alexbeutel.com/webgl/voronoi.html)
        * [https://www.jasondavies.com/voronoi/](https://www.jasondavies.com/voronoi/)
* **入门文章/教程:**
    * Wikipedia: "Voronoi diagram", "Delaunay triangulation"
    * Computational Geometry Algorithms Library (CGAL): [https://www.cgal.org/](https://www.cgal.org/) (更偏向算法实现)
* **书籍 (进阶):**
    * "Computational Geometry: Algorithms and Applications" by Mark de Berg, Otfried Cheong, Marc van Kreveld, Mark Overmars. (经典教材)

## 9. 总结

Voronoi 图是一种强大而直观的工具，用于理解和分析空间中点的邻近关系。它通过将空间划分为由最近种子点定义的区域，为解决各种实际问题提供了基础。虽然其背后的算法可能有些复杂，但理解其核心概念和性质对于初学者来说是最重要的。希望这份笔记能帮助你开启 Voronoi 图的学习之旅！
