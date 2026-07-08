A novel stock trading strategy based on double deep Q-network with sentiment integration
Author links open overlay panel
Xiwen Qin a b
, 
Jiawei Shen a b
, 
Dingxin Xu a b
, 
Siqi Zhang a b

Show more

Add to Mendeley

Share

Cite
https://doi.org/10.1016/j.ins.2025.122541
Get rights and content
Abstract
Reinforcement learning (RL) has gained significant attention in stock trading strategies. However, existing RL models still show shortcomings. On the one hand, they fail to adequately account for the complex factors in real-world markets; on the other hand, they struggle to accurately capture the dynamic nature of financial markets, resulting in limited drawdown control and suboptimal returns. To address these challenges, we propose a novel stock trading strategy based on a Double Deep Q-Network (DDQN) with sentiment integration. First, sentiment features extracted from social media are combined with technical indicators to enhance the model’s understanding of market dynamics. Subsequently, trading decisions are made using the DDQN framework, which learns optimal policies through interaction with the market environment. To enhance performance, we adopt a Convolutional Neural Network − Bidirectional Gated Recurrent Unit (CNN–BiGRU) architecture as the Q-network, where CNN extracts local price patterns for short-term fluctuations, while BiGRU models temporal dependencies to capture long-term trends. Finally, trading signals from the RL process serve as labels to train multiple supervised classifiers. Experiments show that the proposed framework surpasses baseline models in major performance metrics including return, payoff ratio, and Sharpe ratio. This approach aims to provide accurate trading decision support for investors.
Introduction
Financial markets pose an intimidating challenge to forecasting and decision-making because of the nonlinear, complicated dynamics [1]. Asset prices are influenced by an intersection of causes—anything from economic metrics and geopolitical events to the moods of investors—leading to unpredictable behaviors that cannot be easily modelled [2,3]. Conventional theories of finance, including the Efficient Market Hypothesis, assume that markets rapidly incorporate information into prices, leaving little room to achieve persistent outperformance [4]. Actually, markets tend to feature inefficiencies and persistent anomalies, particularly during extreme sentiment [5]. This has spurred increased interest in the use of artificial intelligence methodologies to deal with stock trading [6], as data-driven models may detect latent patterns that are inaccessible to linear models or the design of arbitrary rules by humans. Certainly, Machine Learning (ML) and deep learning methods have become widely deployed in trading and asset management [7], achieving higher returns in certain instances than the use of classical techniques or fundamental considerations alone. Traditional rule-based trading techniques (for instance, fixed thresholds on indicators or heuristic expert systems) find it challenging to deal with the nonlinear market behavior and regime change [8]. A static rule, which works well during one market regime, does not function well once the regime is altered. On the contrary, ML models learn from existing data and can detect subtle patterns between market variables [9]. For instance, methods like the support vector machines and the use of neural networks were employed to forecast stock price movements [10], and these tend to surpass naive benchmarks [11]. These data-driven approaches mark a significant advance over purely theory-driven trading, yet they are not without limitations. Financial time series are noisy, non-stationary, and influenced by myriad hidden factors, which complicates pattern recognition. Empirical studies note that stock data are filled with noise and unstable relationships, involving many interrelated and unobservable influences. As a result, a model that fits past data well might fail to account for a critical factor when new conditions arise. Moreover, most ML predictors operate in a supervised learning setting, training on past examples of market outcomes. Such models are prone to overfitting idiosyncrasies in the training period, which undermines their generalization to future data. In short, while ML and deep learning have improved financial forecasting by modeling complex patterns, their direct application to trading decisions remains challenging due to the high dimensionality and uncertainty inherent in markets.
Reinforcement learning (RL) offers a compelling solution for tackling several of these challenges [12]. In an RL-based trading system, the problem is formulated as a sequential decision-making task: an autonomous agent interacts with the market by choosing actions (buy, sell, hold) and learns a policy that maximizes cumulative rewards (e.g. profit or risk-adjusted return) [13]. This paradigm is appealing because it shifts the focus from one-step prediction to long-term performance optimization. Unlike supervised models that require labeled examples of “correct” decisions, an RL agent can learn directly from feedback in the form of rewards, exploring various strategies to discover profitable patterns. Prior research has demonstrated that deep RL algorithms can, under certain conditions, outperform static strategies by dynamically adapting to market changes. The success of RL in game-playing (e.g., Atari or Go) showcases its ability to master complex sequences of decisions, which has spurred attempts to apply similar techniques to financial trading [14]. If designed well, an RL trader can theoretically learn when to enter or exit positions to maximize returns while managing risks, all without explicit human-coded rules.
Nevertheless, deploying reinforcement learning on real-world financial markets is not straightforward. While games are well-posed, meaning that the state is completely observable and the environment is stable, markets are extremely noisy, partially observable, and affected by unobserved factors such as sentiment or supply–demand disequilibria, and the trading environment is akin to a partial observable Markov decision process [15]. The common standard RL algorithms, which assume stable and completely observable environments, tend to fit noise or generate unstable policies. Furthermore, financial markets are non-stationary; regime shifts and unexpected events cause an earlier learned policy to become useless. Practical limitations such as transaction fees and risk exposure complicate learning—without explicit modeling, the agent can overtrade marginal returns that are offset by fees. As such, straightforwardly implementing vanilla RL tends to lead to policies performing well in simulations but not in actual markets [16].
In light of these limitations, there is a need to augment reinforcement learning-based trading models with domain knowledge and more adaptive architectures. One critical source of information in financial markets is investor sentiment [17]. Emotions and expectations, as captured through news articles, social media, and other sentiment indicators, have a tangible impact on market behavior [18]. Sudden shifts in public sentiment can trigger momentum or reversal effects that are not evident from price history alone. Avoiding consideration of this feature dismisses an essential explanatory variable for price movements. In fact, it is now understood that sentiment is at the core of investment decisions. Recent empirical evidence shows that negative sentiment expressed on social media platforms is significantly associated with lower stock returns, while positive sentiment has limited, indicating an asymmetric impact of sentiment on prices that aligns with behavioral theories such as loss aversion [19]. Attend to sentiment analysis within trading models to estimate the market’s emotional atmosphere—for example, prevailing optimism v. fear—informing decision context significantly. Moreover, to handle sequence and partially observable data effectively, the internal model architecture needs to utilize techniques preserving short-term patterns and long-term dependencies. Convolutional Neural Networks (CNNs) can abstract features at localities of the time-series input (for example, short-term trends or chart patterns) [20], but recurrent networks such as Long Short-Term Memory (LSTM) or Gated Recurrent Unit (GRU) remember previous states to inform subsequent decisions. Specifically, a Bidirectional GRU (BiGRU) can handle time-series data in both forward and reverse directions, offering richer context at each time step [21]. By integrating CNN and BiGRU into the neural network of an RL agent, we provide the agent with learning stronger state representations to take into consideration the very nuanced temporal structure and extreme market mood shifts. Moreover, leveraging an enhanced RL algorithm such as Double Deep Q-Network (DDQN) reduces problems such as the overestimation bias related to learning values to provide further stable training [22].
In response to the previously described problems, in this paper, the authors suggest an intelligent trading approach combining market sentiment and CNN-BiGRU-based deep reinforcement learning. Overall, the primary contributions of the present work are as follows:
(1)
Multi-factor Trading scheme: A new trading scheme is established through the smooth combination of standard technical indicators with sentiment indicators derived from market data. By combining technical and textual data, the scheme has access to both quantitative trends and qualitative sentiment about the market and offers a broader representation of the market state than using one-factor methodologies.
(2)
CNN-BiGRU-Augmented DDQN Framework: A Double Deep Q-network-based deep reinforcement learning framework is formulated, enhanced with the integration of the CNN-BiGRU neural architecture. The CNN extracts local patterns in the input features (for instance, short-term price ranges or indicator variations), and the bidirectional GRU handles temporal dependencies as well as contextual associations. This synergy allows the agent to characterize intricate, time-sensitive market patterns and improves decision-making resilience.
(3)
Reward Function with Transaction Costs: A carefully crafted reward function is introduced, explicitly accounting for transaction costs. By penalizing excessive trading activities and incorporating costs into the reward signal, the reinforcement learning agent is incentivized to favor strategies that are not only profitable but also cost-efficient and practically executable, thereby avoiding high-frequency trading behavior that could erode returns.
(4)
Empirical Performance and Robustness: A series of extensive experiments were conducted using real-world stock market data to validate the effectiveness of the proposed method against state-of-the-art benchmarks. The findings indicate that the sentiment-enhanced DDQN strategy delivers superior profitability and achieves better risk-adjusted returns compared to baseline models.
The following sections outline the structure of the paper: Section 2 reviews related literature on trading systems, including supervised learning methods, reinforcement learning trading systems and sentiment-integrated reinforcement learning models. Section 3 describes the fundamental models used in this study. Section 4 presents the problem formulation, explains key financial market concepts, and introduces the proposed multi-factor trading framework. Section 5 explains the experimental setup, evaluation metrics, and empirical results, followed by a discussion on the model’s performance. Section 6 wraps up the paper and outlines potential directions for future research.
Access through your organization
Check access to the full text by signing in through your organization.

Section snippets
Machine learning trading system
In recent years, with the rapid development of deep learning technologies, an increasing number of models have been applied to financial trading, aiming to identify optimal buy and sell timings to generate profit. Among various approaches, supervised learning remains one of the most widely adopted strategies. Some studies frame stock prediction as a regression task, attempting to accurately forecast future prices [23], while others approach it as a classification task, focusing on the
Convolutional neural network
Convolutional Neural Network (CNN) is an architecture of deep learning that has extensive use in applications ranging from image processing and speech recognition to natural language processing. The basic principle behind CNN is to learn features automatically from raw input data just as the human visual system processes information. CNNs differ from regular fully connected networks as they use dedicated architectures that combine convolution and the use of pooling, enabling them to extract
Stock market environment and overall framework
In this study, the computation of various stock technical indicators is conducted based on the TA-Lib library. The stock trading task is modeled as a Markov Decision Process (MDP), consisting of the following components: state, action, reward, policy, and Q-value. A total of 20 stocks from the SSE 50 Index and 20 stocks from the SZSE 100 Index are selected as trading targets. The agent observes information related to these 40 stocks and performs trading operations including buying, selling, and 
Dataset and experimental setting
The selected dataset consists of daily trading data for 40 stocks listed on the Shanghai and Shenzhen Stock Exchanges, covering the period from January 1, 2011, to November 29, 2024. All data are obtained automatically through the Tushare quantitative data platform by registering on its official website and acquiring a free token. The dataset includes the open, high, low, and close prices, as well as trading volume for each trading day.
After completing the data fusion and normalization
Conclusion
The paper develops an information-fused multi-modal stock trading system that couples the Double Deep Q-Network (DDQN) with the CNN-BiGRU architecture, which improves the capability of the model to learn the local movements and temporal features from the sequences of stock prices. In constructing the reinforcement learning environment, both sentiment scores and technical indicators were incorporated to enable the model to learn how changes in market sentiment influence trading behavior, thereby 
CRediT authorship contribution statement
Xiwen Qin: Writing – review & editing, Supervision. Jiawei Shen: Writing – original draft, Conceptualization. Dingxin Xu: Supervision, Methodology. Siqi Zhang: Methodology, Conceptualization.
Declaration of competing interest
The authors declare that they have no known competing financial interests or personal relationships that could have appeared to influence the work reported in this paper.
Acknowledgement
We deeply appreciate the funding from the National Social Science Fund of China projects (Grant Nos. 23BTJ047, 24BTJ074), the Jilin Provincial Department of Science and Technology projects (Grant Nos. 20210101149JC, 20200403182SF, YDZJ202501ZYTS657), and the Jilin Provincial Department of Education (Grant No. JJKH20240843CY). The authors express their gratitude to the journal editor and reviewers for their insightful comments throughout the revision process. Your expert advice and comprehensive 
References (42)
L. Pan et al.
Stock market development and economic growth: empirical evidence from China
Econ. Model.
(2018)
E.A. Gerlein et al.
Evaluating machine learning classification for financial trading: an empirical approach
Expert Syst. Appl.
(2016)
D. Zhang et al.
The application research of neural network and BP algorithm in stock price pattern classification and prediction
Future Gener. Comput. Syst.- Int. J. Escience
(2021)
W.-C. Lin et al.
Factors affecting text mining based stock prediction: text feature representations, machine learning models, and news platforms
Appl. Soft Comput.
(2022)
Y. Yu et al.
Novel optimization approach for realized volatility forecast of stock price index based on deep reinforcement learning model
Expert Syst. Appl.
(2023)
Y.-F. Chen et al.
Sentiment-influenced trading system based on multimodal deep reinforcement learning
Appl. Soft Comput.
(2021)
L. Avramelou et al.
Deep reinforcement learningfor financial trading using multi-modal features
Expert Syst. Appl.
(2024)
E.C. Zabor et al.
Logistic regression in clinical studies
Int. J. Radiat. Oncol. Biol. Phys.
(2022)
J. Zhang et al.
Insights into geospatial heterogeneity of landslide susceptibility based on the SHAP-XGBoost model
J. Environ. Manage.
(2023)
P. Arestis et al.
the financial development and growth nexus: a meta-analysis
J. Econ. Surv.
(2015