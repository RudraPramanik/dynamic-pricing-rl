Sustainable supply chain management: A green computing approach using deep Q-networks
Author links open overlay panel
Di Yuan
, 
Yue Wang

Show more

Add to Mendeley

Share

Cite
https://doi.org/10.1016/j.suscom.2024.101063
Get rights and content
Highlights
•
Developed an eco-efficient supply chain optimization model using Deep Q-Networks.
•
The ISCO-DQ model reduces inventory costs and carbon emissions by over 10 %.
•
Integrates Markov Decision Processes for energy-efficient inventory management.
•
Achieves rapid adaptation to fluctuating demand, enhancing system sustainability.
Abstract
This paper addresses the challenges of resource allocation and inventory management in supply chain systems by constructing an intelligent supply chain optimization model based on Deep Q-Networks (ISCO-DQ), emphasizing eco-efficiency. Initially, the study builds a supply chain model that incorporates supplier-customer relationships, guided by the principles of green computing to minimize environmental impact. The model applies Markov Decision Processes to develop a framework for sustainable supplier inventory control, focusing on reducing waste and optimizing resource usage. Utilizing the function approximation capabilities of Deep Q-Networks, the model not only achieves intelligent resource allocation but also prioritizes energy-efficient practices in inventory management. Experimental results indicate that the ISCO-DQ inventory control model converges to approximately −41,400 and −181,300 after around 100 and 300 cycles, respectively, under customer demand distributions that follow normal distributions. Furthermore, compared to traditional single-period stochastic and fixed-order quantity inventory control models, the total cost of the ISCO-DQ model is reduced by an average of 6.7 % and 16 %, respectively, while minimizing carbon emissions associated with overproduction and excess inventory. Additionally, the ISCO-DQ model significantly mitigates costs arising from demand uncertainty by quickly adapting to fluctuations and optimizing inventory strategies, thereby fostering a circular economy. This demonstrates that the ISCO-DQ inventory control model effectively addresses inefficiencies, inflexibility, and suboptimal resource allocation in conventional supply chain management, ultimately promoting sustainable development and environmental stewardship for enterprises.
Introduction
In today's era of rapid globalization and information technology advancement, supply chain management has emerged as a crucial factor for enhancing enterprise competitiveness and ensuring sustainable development. Supply chain management encompasses various stages, including raw material procurement, manufacturing, logistics, and distribution. The coordination and collaboration between these stages directly impact an enterprise's operational efficiency and cost control [1]. Particularly in inventory management and resource allocation, excessively high inventory levels can inflate operational costs, whereas excessively low inventory can risk stockouts and affect customer satisfaction. Additionally, the logistics segment of the supply chain faces challenges such as high transportation costs and slow delivery times. Given the increasingly complex market environment and rapidly changing demand, under the traditional supply chain model, issues such as irrational resource allocation and high inventory costs have become prominent, severely impacting the sustainability of enterprises. According to recent research, poor inventory management can lead to an increase in corporate costs by 7–16 %. Therefore, exploring intelligent supply chain optimization solutions is of great significance for promoting the sustainable development of enterprises[2].
Deep reinforcement learning (DRL) [3] integrates the perceptual strengths of deep learning with the intelligent decision-making capabilities of reinforcement learning, offering a novel approach to addressing complex and evolving real-world problems. In supply chain management, DRL enables the development and refinement of optimal resource allocation and inventory management strategies by simulating the interaction between the intelligent agent and the environment (i.e., the supply chain system) through continuous trial, error, and optimization [4]. This self-adaptive and dynamically adjustable capability allows the supply chain to respond rapidly and maintain high efficiency and flexibility amidst market fluctuations, demand changes, and other uncertainties.
Despite the promising potential of DRL in optimizing supply chains, challenges persist in refining inventory management strategies. Current algorithms predominantly concentrate on enhancing resource allocation efficiency, yet achieving an optimal equilibrium between curtailing inventory costs and mitigating the risk of stockouts remains elusive. Furthermore, accurately capturing and addressing the diverse needs of customers, optimizing procurement and replenishment strategies, improving inventory turnover, and promoting green recycling and sustainable resource utilization represent significant hurdles for DRL in advancing sustainable supply chain development.
To address these issues, this study aims to develop an intelligent supply chain optimization system leveraging deep reinforcement learning techniques, thereby enhancing supply chain efficiency and flexibility while fostering sustainable enterprise growth. The specific contributions of this paper are as follows:
•
Supply Chain Model Construction: This paper constructs a comprehensive supply chain model, analyzing from the perspective of supplier configuration and supplier-customer relationships. It determines the total cost of the supply chain, thereby providing a robust foundation for subsequent supply chain scheduling optimization and inventory management implementation.
•
Modeling Supplier Inventory Control: The study formulates the supplier inventory control decision problem using a Markov decision process. It utilizes the state spaces of periods t-2, t-1, and t as model inputs to address the dimensionality increase of the state space over time and the latency between the intelligence's ordering decisions and observed inventory costs.
•
Development of the Intelligent Supply Chain Optimization Model (ISCO-DQ): This paper introduces the ISCO-DQ model, an intelligent supply chain optimization model based on deep Q-networks. By incorporating the deep Q-network model into the Markov decision-making process, it enables approximate estimation of the action value function. Ultimately, it leverages the state space observed by the energy transmitting entity to output the ordering decisions made by the intelligent entity, thereby achieving efficient resource allocation.
This paper reviewed the current research on reinforcement learning, deep learning, and deep reinforcement learning in supply chain optimization in Section 2. Section 3 presented the constructed supply chain model and the ISCO-DQ model. Section 4 discussed experimental results and analyze the performance of the ISCO-DQ model in comparison with existing inventory control models. Section 5 summarized the findings and discussed future research directions