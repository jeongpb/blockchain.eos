# EOS.IO Technical White Paper
# EOS.IO 기술 백서

**DRAFT: June 5, 2017**
**초안 작성일: 2017년 6월 5일**

**Abstract:** The EOS.IO software introduces a new blockchain architecture designed to enable vertical and horizontal scaling of decentralized applications. This is achieved by creating an operating system-like construct upon which applications can be built. The software provides accounts, authentication, databases, asynchronous communication and the scheduling of applications across hundreds of CPU cores or clusters. The resulting technology is a blockchain architecture that scales to millions of transactions per second, eliminates user fees, and allows for quick and easy deployment of decentralized applications.

**초록:** EOS.IO 소프트웨어는 탈중앙화 애플리케이션의 수직 및 수평 확장이 가능하도록 디자인된 새로운 블록체인 아키텍처를 소개합니다. 이는 애플리케이션을 구축할 수 있는 운영체제와 유사한 구조를 생성함으로 완성됩니다. 본 소프트웨어는 수백 개의 CPU 코어 또는 클러스터에 계정(accounts), 인증(authentication), 데이터베이스(databases), 비동기 통신(asynchronous communication), 애플리케이션 스케쥴링(application scheduling) 기능을 제공합니다. 그 결과 초당 수백만 건의 트랜잭션 처리 능력을 갖추면서도, 수수료가 없고, 빠르고 쉽게 애플리케이션을 개발할 수 있는 블록체인 아키텍처 기술이 탄생했습니다.

Copyright © 2017 block.one

저작권 소유 © 2017 block.one

Without permission, anyone may use, reproduce or distribute any material in this whitepaper for non-commercial and educational use (i.e., other than for a fee or for commercial purposes) provided that the original source and the applicable copyright notice are cited.

누구든지 허가 없이 원래의 출처와 해당 저작권 고지가 언급된 경우 비영리적이고 교육적인 용도 (즉, 유료 또는 상업적 목적 이외의 목적)로 본 백서의 자료를 사용, 복제 또는 배포할 수 있습니다.

**DISCLAIMER:**  This draft EOS.IO Technical Whitepaper is for information purposes only. block.one does not guarantee the accuracy of the conclusions reached in this paper, and the whitepaper is provided “as is” with no representations and warranties, express or implied, whatsoever, including, but not limited to: (i) warranties of merchantability, fitness for a particular purpose, title or noninfringement; (ii) that the contents of this whitepaper are free from error or suitable for any purpose; and (iii) that such contents will not infringe third-party rights. All warranties are expressly disclaimed. block.one and its affiliates expressly disclaim all liability for and damages of any kind arising out of the use, reference to, or reliance on any information contained in this whitepaper, even if advised of the possibility of such damages.  In no event will block.one or its affiliates be liable to any person or entity for any direct, indirect, special or consequential damages for the use of, reference to, or reliance on this whitepaper or any of the content contained herein.

**면책 조항:** EOS.IO 기술 백서 초안은 오직 정보 제공의 목적으로써 제공됩니다. block.one은 이 백서에서 도달한 결론의 정확성을 보장하지 않으며, 백서는 "있는 그대로" 제공되며 이는 (단, 이에 한정되지는 않음) 명시적이거나 묵시적인 것으로서 어떠한 보증도 하지 않습니다. (i) 상품성에 대한 보증, 특정 목적을 위한 적합성, 타이틀 또는 법규의 위반이 없음; (ii) 본 백서의 내용에 오류가 없거나 어떤 목적에 적합하다는 것; (iii) 그러한 내용이 제3자의 권리를 침해하지 않을 것입니다. 명시적으로 어떠한 보증도 되지 않습니다. block.one과 그 계열사는 이 백서에 포함된 정보의 사용, 참조 또는 신뢰로 인해 발생하는 모든 종류의 손해에 대해 명시적으로 책임을 지지 않습니다. 어떤 경우에도 본 백서 또는 여기에 포함된 내용의 사용, 참조 또는 의존에 대한 직접적, 간접적, 특수적 또는 결과적 손해에 대해 어떠한 개인이나 단체에 대해서도 책임을 지지 않습니다.

<!-- TOC -->

- [Background](#background)
- [EOS.IO 탄생 배경 (Background)](#eosio-탄생-배경-background)
- [Requirements for Blockchain Applications](#requirements-for-blockchain-applications)
- [블록체인 애플리케이션의 요구사항 (Requirements for Blockchain Application)](#블록체인-애플리케이션의-요구사항-requirements-for-blockchain-application)
    - [Support Millions of Users](#support-millions-of-users)
    - [1. 수백만의 사용자 지원 (Support Millions of Users)](#1-수백만의-사용자-지원-support-millions-of-users)
    - [Free Usage](#free-usage)
    - [2. 무료 사용 (Free Usage)](#2-무료-사용-free-usage)
    - [Easy Upgrades and Bug Recovery](#easy-upgrades-and-bug-recovery)
    - [3. 간편한 업그레이드 및 버그 해소 (Easy upgrades and Bug Recovery)](#3-간편한-업그레이드-및-버그-해소-easy-upgrades-and-bug-recovery)
    - [Low Latency](#low-latency)
    - [4. 짧은 지연 시간 (Low Latency)](#4-짧은-지연-시간-low-latency)
    - [Sequential Performance](#sequential-performance)
    - [5. 순차(sequential) 처리 성능 (Sequential Performance)](#5-순차sequential-처리-성능-sequential-performance)
    - [Parallel Performance](#parallel-performance)
    - [6. 병렬 처리 성능 (Parallel Performance)](#6-병렬-처리-성능-parallel-performance)
- [Consensus Algorithm (DPOS)](#consensus-algorithm-dpos)
- [합의 알고리즘 (DPOS) (Consensus Algorithm)](#합의-알고리즘-dpos-consensus-algorithm)
    - [Transaction Confirmation](#transaction-confirmation)
    - [트랜잭션 확인 (Transaction Confirmation)](#트랜잭션-확인-transaction-confirmation)
    - [Transaction as Proof of Stake (TaPoS)](#transaction-as-proof-of-stake-tapos)
    - [트랜잭션 기반 지분 증명 (Transaction as Proof of Stake, TaPoS)](#트랜잭션-기반-지분-증명-transaction-as-proof-of-stake-tapos)
- [Accounts](#accounts)
- [계정 (Accounts)](#계정-accounts)
    - [Messages & Handlers](#messages--handlers)
    - [메시지와 처리기 (Messages & Handlers)](#메시지와-처리기-messages--handlers)
    - [Role Based Permission Management](#role-based-permission-management)
    - [역할 기반 권한 관리 (Role Based Permission Management)](#역할-기반-권한-관리-role-based-permission-management)
        - [Named Permission Levels](#named-permission-levels)
        - [명명된 권한 수준 (Named Permission Levels)](#명명된-권한-수준-named-permission-levels)
        - [Named Message Handler Groups](#named-message-handler-groups)
        - [명명된 메시지 처리기 그룹 (Named Message Handler Groups)](#명명된-메시지-처리기-그룹-named-message-handler-groups)
        - [Permission Mapping](#permission-mapping)
        - [권한 매핑 (Permission Mapping)](#권한-매핑-permission-mapping)
        - [Evaluating Permissions](#evaluating-permissions)
        - [권한 검사 (Evaluating Permissions)](#권한-검사-evaluating-permissions)
            - [Default Permission Groups](#default-permission-groups)
            - [기본 권한 그룹( Default Permission Groups)](#기본-권한-그룹-default-permission-groups)
            - [Parallel Evaluation of Permissions](#parallel-evaluation-of-permissions)
            - [권한 검사의 병렬화 (Parallel Evaluation of Permissions)](#권한-검사의-병렬화-parallel-evaluation-of-permissions)
    - [Messages with Mandatory Delay](#messages-with-mandatory-delay)
    - [메시지의 필수 지연 시간 (Messages with Mandatory Delay)](#메시지의-필수-지연-시간-messages-with-mandatory-delay)
    - [Recovery from Stolen Keys](#recovery-from-stolen-keys)
    - [키 도난 상태에서의 복구 (Recovery from Stolen Keys)](#키-도난-상태에서의-복구-recovery-from-stolen-keys)
- [Deterministic Parallel Execution of Applications](#deterministic-parallel-execution-of-applications)
- [애플리케이션의 결정론적 병렬 실행 (Deterministic Parallel Execution of Applications)](#애플리케이션의-결정론적-병렬-실행-deterministic-parallel-execution-of-applications)
    - [Minimizing Communication Latency](#minimizing-communication-latency)
    - [통신 지연 최소화 (Minimizing Communication Latency)](#통신-지연-최소화-minimizing-communication-latency)
    - [Read-Only Message Handlers](#read-only-message-handlers)
    - [읽기 전용 메시지 처리기 (Read-Only Message Handlers)](#읽기-전용-메시지-처리기-read-only-message-handlers)
    - [Atomic Transactions with Multiple Accounts](#atomic-transactions-with-multiple-accounts)
    - [다중 계정의 원자적 트랜잭션 (Atomic Transactions with Multiple Accounts)](#다중-계정의-원자적-트랜잭션-atomic-transactions-with-multiple-accounts)
    - [Partial Evaluation of Blockchain State](#partial-evaluation-of-blockchain-state)
    - [블록체인 상태의 부분 검사 (Partial Evaluation of Blockchain State)](#블록체인-상태의-부분-검사-partial-evaluation-of-blockchain-state)
    - [Subjective Best Effort Scheduling](#subjective-best-effort-scheduling)
    - [주관적 최선 스케쥴링 (Subjective Best Effort Scheduling)](#주관적-최선-스케쥴링-subjective-best-effort-scheduling)
- [Token Model and Resource Usage](#token-model-and-resource-usage)
- [토큰 모델과 리소스 사용 (Token Model and Resource Usage)](#토큰-모델과-리소스-사용-token-model-and-resource-usage)
    - [Objective and Subjective Measurements](#objective-and-subjective-measurements)
    - [객관적 측정과 주관적 측정 (Objective and Subjective Measurements)](#객관적-측정과-주관적-측정-objective-and-subjective-measurements)
    - [Receiver Pays](#receiver-pays)
    - [수취인 부담 (Receiver Pays)](#수취인-부담-receiver-pays)
    - [Delegating Capacity](#delegating-capacity)
    - [리소스 허용량 위임 (Delegating Capacity)](#리소스-허용량-위임-delegating-capacity)
    - [Separating Transaction costs from Token Value](#separating-transaction-costs-from-token-value)
    - [토큰의 가치와 트랜잭션 비용의 분리 (Separating Transaction costs from Token Value)](#토큰의-가치와-트랜잭션-비용의-분리-separating-transaction-costs-from-token-value)
    - [State Storage Costs](#state-storage-costs)
    - [상태 저장 비용 (State Storage Costs)](#상태-저장-비용-state-storage-costs)
    - [Block Rewards](#block-rewards)
    - [블록 보상 (Block Rewards)](#블록-보상-block-rewards)
    - [Community Benefit Applications](#community-benefit-applications)
    - [커뮤니티 혜택 애플리케이션 (Community Benefit Applications)](#커뮤니티-혜택-애플리케이션-community-benefit-applications)
- [Governance](#governance)
- [거버넌스 (Governance)](#거버넌스-governance)
    - [Freezing Accounts](#freezing-accounts)
    - [계정 동결 (Freezing Accounts)](#계정-동결-freezing-accounts)
    - [Changing Account Code](#changing-account-code)
    - [계정 코드 변경 (Changing Account Code)](#계정-코드-변경-changing-account-code)
    - [Constitution](#constitution)
    - [약관 (Constitution)](#약관-constitution)
    - [Upgrading the Protocol & Constitution](#upgrading-the-protocol--constitution)
    - [프로토콜과 약관의 개정 (Upgrading the Protocol & Constitution)](#프로토콜과-약관의-개정-upgrading-the-protocol--constitution)
        - [Emergency Changes](#emergency-changes)
        - [응급 변경 (Emergency Changes)](#응급-변경-emergency-changes)
- [Scripts & Virtual Machines](#scripts--virtual-machines)
- [스크립트와 가상 머신 (Scripts & Virtual Machines)](#스크립트와-가상-머신-scripts--virtual-machines)
    - [Schema Defined Messages](#schema-defined-messages)
    - [스키마 정의 메시지 (Schema Defined Messages)](#스키마-정의-시지-schema-defined-messages)
    - [Schema Defined Database](#schema-defined-database)
    - [스키마 정의 데이터베이스 (Schema Defined Database)](#스키마-정의-데이터베이스-schema-defined-database)
    - [Separating Authentication from Application](#separating-authentication-from-application)
    - [애플리케이션과 인증 분리 (Separating Authentication from Application)](#애플리케이션과-인증-분리-separating-authentication-from-application)
    - [Virtual Machine Independent Architecture](#virtual-machine-independent-architecture)
    - [가상 머신 독립 아키텍처 (Virtual Machine Independent Architecture)](#가상-머신-독립-아키텍처-virtual-machine-independent-architecture)
        - [Web Assembly (WASM)](#web-assembly-wasm)
        - [웹어셈블리 (WASM; Web Assembly)](#웹어셈블리-wasm-web-assembly)
        - [Ethereum Virtual Machine (EVM)](#ethereum-virtual-machine-evm)
        - [이더리움 가상 머신 (EVM; Ethereum Virtual Machine)](#이더리움-가상-머신-evm-ethereum-virtual-machine)
- [Inter Blockchain Communication](#inter-blockchain-communication)
- [블록체인 간 통신 (Inter Blockchain Communication)](#블록체인-간-통신-inter-blockchain-communication)
    - [Merkle Proofs for Light Client Validation (LCV)](#merkle-proofs-for-light-client-validation-lcv)
    - [경량화된 클라이언트 검증(LCV)을 위한 머클 증명 (Merkle Proofs for Light Client Validation)](#경량화된-클라이언트-검증lcv을-위한-머클-증명-merkle-proofs-for-light-client-validation)
    - [Latency of Interchain Communication](#latency-of-interchain-communication)
    - [체인 간 통신의 지연 시간 (Latency of Interchain Communication)](#체인-간-통신의-지연-시간-latency-of-interchain-communication)
    - [Proof of Completeness](#proof-of-completeness)
    - [완전성 증명 (Proof of Completeness)](#완전성-증명-proof-of-completeness)
- [Conclusion](#conclusion)
- [결론 (Conclusion)](#결론-conclusion)

# Background

# EOS.IO 탄생 배경 (Background)

Blockchain technology was introduced in 2008 with the launch of the bitcoin currency, and since then entrepreneurs and developers have been attempting to generalize the technology in order to support a wider range of applications on a single blockchain platform.  

블록체인 기술은 2008년 비트코인 화폐의 출현과 함께 시작되었으며, 이후 기업가와 개발자는 하나의 블록체인 플랫폼에서 다양한 애플리케이션을 지원하기 위해 기술의 일반화를 시도해 왔습니다.

While a number of blockchain platforms have struggled to support functional decentralized applications, application specific blockchains such as the BitShares decentralized exchange (2014) and Steem social media platform (2016) have become heavily used blockchains with tens of thousands of daily active users. They have achieved this by increasing performance to thousands of transactions per second, reducing latency to 1.5 seconds, eliminating fees, and providing a user experience similar to those currently provided by existing centralized services.

다수의 블록체인 플랫폼은 제대로 작동하는 탈중앙화 애플리케이션을 지원하기 위해 노력하는 동안, BitShares 탈중앙화 거래소(2014) 및 Steem 소셜 미디어 플랫폼(2016)과 같은 애플리케이션 특화 블록체인은 이미 수만 명의 일일 사용자를 가진 블록체인으로 성장하였습니다. 이는 초당 수천 건의 트랜잭션 지원과 1.5초의 지연시간과 같은 성능 향상, 사용 수수료의 제거, 현재 서비스되는 중앙 집중형 서비스와 유사한 수준의 사용자 경험 제공을 통해 가능하게 되었습니다.

Existing blockchain platforms are burdened by large fees and limited computational capacity that prevent widespread blockchain adoption.

현존하는 블록체인 플랫폼들은 비싼 수수료와 연산능력의 한계 때문에 블록체인의 광범위한 사용에 있어서 어려움을 겪고 있습니다.

# Requirements for Blockchain Applications

# 블록체인 애플리케이션의 요구사항 (Requirements for Blockchain Application)

In order to gain widespread use, applications on the blockchain require a platform that is flexible enough to meet the following requirements:

블록체인 위에서 돌아가는 애플리케이션이 대중적으로 사용되기 위해서는 다음의 요구사항을 만족하는 유연한 플랫폼을 갖춰야 합니다.

## Support Millions of Users

## 1. 수백만의 사용자  (Support Millions of Users)

Disrupting businesses such as Ebay, Uber, AirBnB, and Facebook, require blockchain technology capable of handling tens of millions of active daily users.  In certain cases, applications may not work unless a critical mass of users is reached and therefore a platform that can handle mass number of users is paramount.

Ebay, Uber, AirBnB, Facebook과 같은 기존 서비스와 경쟁력을 갖추기 위해서 수천만의 일일 사용자를 수용할 수 있는 블록체인 기술이 필요합니다. 또한 많은 사용자가 이용하지 않을 경우 작동하지 않는 애플리케이션도 있으므로 많은 사용자를 수용하는 플랫폼은 무엇보다 중요합니다.


## Free Usage

## 2. 무료 사용 (Free Usage)

Application developers need the flexibility to offer users free services; Users should not have to pay in order to use the platform or benefit from its services. A blockchain platform that is free to use for users will likely gain more widespread adoption. Developers and businesses can then create effective monetization strategies.

애플리케이션 개발자는 사용자에게 무료로 서비스 할 수 있어야 합니다. 사용자는 플랫폼의 이용과 서비스의 혜택을 무료로 누릴 수 있어야 합니다. 사용자가 무료로 이용할 수 있는 블록체인 플랫폼이 더 널리 전파될 것입니다. 빠른 대중화로 인해 기업가와 개발자는 효율적인 수익 창출 전략을 만들어 낼 수 있을 것입니다.

## Easy Upgrades and Bug Recovery

## 3. 간편한 업그레이드 및 버그 해소 (Easy upgrades and Bug Recovery)

Businesses building blockchain based applications need the flexibility to enhance their applications with new features.

블록체인 기반 애플리케이션을 만드는 기업은 그들의 애플리케이션에 새로운 기능을 추가하고 개선할 수 있어야 합니다.

All non-trivial software is subject to bugs, even with the most rigorous of formal verification. The platform must be robust enough to fix bugs when they inevitably occur.

많은 소프트웨어들은 엄격한 공식적인 검사를 진행함에도 버그가 발생합니다. 플랫폼은 애플리케이션에서 버그가 발생하였을 때 버그를 수정할 수 있을 만큼 안정적이어야 합니다.

## Low Latency

## 4. 짧은 지연 시간 (Low Latency)

A good user experience demands reliable feedback with delay of no more than a few seconds. Longer delays frustrate users and make applications built on a blockchain less competitive with existing non-blockchain alternatives.

좋은 사용자 경험은 수 초 이하의 지연시간을 통한 안정적인 피드백을 필요로 합니다. 긴 지연 시간은 사용자의 불만을 일으키며, 이러한 블록체인 애플리케이션은 블록체인을 사용하지 않는 기성 시장의 애플리케이션에 비해 경쟁력이 떨어집니다.

## Sequential Performance

## 5. 순차(sequential) 처리 성능 (Sequential Performance)

There are some applications that just cannot be implemented with parallel algorithms due to sequentially dependent steps.  Applications such as exchanges need enough sequential performance to handle high volumes and therefore a platform with fast sequential performance is required.

몇몇 애플리케이션은 순차적인 처리 단계를 거쳐야 하기 때문에 병렬 알고리즘으로 구현될 수 없습니다. 거래소(exchange)와 같은 애플리케이션들은 많은 양의 거래를 순차적으로 처리하는 충분한 성능을 요구하므로, 플랫폼은 빠른 순차 처리 성능이 필요합니다.

## Parallel Performance

## 6. 병렬 처리 성능 (Parallel Performance)

Large scale applications need to divide the workload across multiple CPUs and computers.

거대 규모의 애플리케이션은 하나의 작업을 다수의 CPU와 컴퓨터에 분배하여 처리할 수 있어야 합니다.

# Consensus Algorithm (DPOS)

# 합의 알고리즘 (DPOS) (Consensus Algorithm)

EOS.IO software utilizes the only decentralized consensus algorithm capable of meeting the performance requirements of applications on the blockchain, [Delegated Proof of Stake (DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper). Under this algorithm, those who hold tokens on a blockchain may select block producers through a continuous approval voting system and anyone may choose to participate in block production and will be given an opportunity to produce blocks proportional to the total votes they have received relative to all other producers. For private blockchains the management will use the tokens to add and remove IT staff.

EOS.IO 소프트웨어는 블록체인 애플리케이션의 성능 요구사항을 충족할 수 있는 유일한 탈중앙화 합의 알고리즘인 [지분 위임 증명(DPOS; Deleteged Proof-Of-Stake)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper)을 사용합니다. 이 알고리즘은 다음과 같이 동작합니다.

블록체인의 토큰을 보유한 사람은 상시 운영되는 투표 시스템을 통해 블록 생산자(block producer)를 선출하며, 누구나 블록 생산자로 참여할 수 있습니다. 블록을 생산할 기회는 다른 생산자들이 받은 전체 투표 수에 대한 본인이 받은 투표 수의 비율에 따라 결정됩니다. 프라이빗 블록체인에서 관리자는 토큰을 사용하여 IT 직원을 추가하거나 제거할 수 있습니다.


The EOS.IO software enables blocks to be produced exactly every 3 seconds and exactly one producer is authorized to produce a block at any given point in time. If the block is not produced at the scheduled time then the block for that time slot is skipped.  When one or more blocks are skipped, there is a 6 or more second gap in the blockchain. If the block is not produced at the scheduled time then the block for that time slot is skipped. When one or more blocks are skipped, there is a 6 or more second gap in the blockchain.

EOS.IO 소프트웨어는 정확히 3초마다 블록이 만들어질 수 있게 하며, 각 시점마다 오직 한 명의 블록 생산자만이 블록을 생성할 수 있습니다. 만약 정해진 시간에 블록이 생산되지 않을 경우 해당 시점의 블록은 무시됩니다. 1개 혹은 그 이상의 블록이 무시될 경우 블록체인에는 6초 혹은 그 이상의 간격(gap)이 나타납니다.

Using the EOS.IO software blocks are produced in rounds of 21. At the start of each round 21 unique block producers are chosen. The top 20 by total approval are automatically chosen every round and the last producer is chosen proportional to their number of votes relative to other producers. The selected producers are shuffled using a pseudorandom number derived from the block time.  This shuffling is done to ensure that all producers maintain balanced connectivity to all other producers.

Using the EOS.IO software blocks are produced in rounds of 21.

EOS.IO 소프트웨어를 이용하여 블록들은 21번의 단계로 구성되는 라운드로 생성되며, 각 라운드가 시작될 때 21명의 블록 생산자가 정해집니다. 라운드마다  많은 득표를 받은 상위 20명의 블록 생산자가 자동으로 배정되며, 마지막 생산자는 다른 생산자와의 상대적인 투표수에 비례하여 선출됩니다.

The selected producers are shuffled using a pseudorandom number derived from the block time. This shuffling is done to ensure that all producers maintain balanced connectivity to all other producers.

블록 시간으로 유도되는 의사 난수(pseudorandom number)에 따라 선출된 생산자들의 블록 생성 순서를 랜덤하게 섞습니다. 블록 생성 순서를 섞는 것은 모든 생산자가 다른 생산자들과 균형적인 연결(balanced connectivity)을 유지하도록 진행합니다.

If a producer misses a block and has not produced any block within the last 24 hours they are removed from consideration until they notify the blockchain of their intention to start producing blocks again. This ensures the network operates smoothly by minimizing the number of blocks missed by not scheduling those who are proven to be unreliable.

생산자가 블록 생성에 실패하고 지난 24시간 동안 어떠한 블록을 생성하지 않는다면, 블록체인에 블록 생성 참여 의사를 알려주기 전까지 후보군에서 제외됩니다. 신뢰할 수 없는 사람을 참가시키지 않으므로 놓치는 블록의 수를 최소화하고 네트워크가 원활하게 동작하도록 보장합니다.

Under normal conditions a DPOS blockchain does not experience any forks because the block producers cooperate to produce blocks rather than compete.  In the event there is a fork, consensus will automatically switch to the longest chain. This metric works because the rate at which blocks are added to a blockchain chain fork is directly correlated to the percentage of block producers that share the same consensus. In other words, a blockchain fork with more producers on it will grow in length faster than one with fewer producers.  Furthermore, no block producer should be producing blocks on two forks at the same time. If a block producer is caught doing this then such block producer will likely be voted out. Cryptographic evidence of such double-production may also be used to automatically remove abusers.

일반적인 상황에서 지분 위임 증명(DPOS) 알고리즘을 사용하는 블록체인은 어떠한 포크(fork)도 일어나지 않습니다. 이는 블록 생성자가 경쟁이 아닌 협력을 하기 때문입니다. 포크가 일어난 경우, 합의 알고리즘은 자동으로 가장 긴 블록 체인(chain)을 선택합니다. 이 방법이 동작하는 이유는, 특정 블록체인 포크에 블록들이 추가되는 속도는 같은 합의를 공유하는 블록 생성자의 비율과 직접 연관되기 때문입니다. 다수의 생산자 존재하는 블록체인 포크는 적은 생산자를 가진 것에 비하여 빠르게 증가합니다. 추가로, 어떠한 블록 생성자도 동시에 두 개의 포크에 블록을 생성할 수 없습니다. 이러한 경우가 적발될 경우 해당 블록 생성자는 탄핵당할 것입니다. 이러한 이중 생산에 대한 암호학적 증거(cryptographic evidence)는 정당하지 않은 방법으로 이득을 취한 사람을 자동으로 제거하는 데 사용될 수 있습니다.


## Transaction Confirmation

## 트랜잭션 확인 (Transaction Confirmation)

Typical DPOS blockchains have 100% block producer participation. A transaction can be considered confirmed with 99.9% certainty after an average of 1.5 seconds from time of broadcast.

일반적인 DPOS 블록체인은 100%의 블록 생산자 참여율을 가집니다. 전파(braodcasting) 시간부터 평균 1.5초의 시간이 흐르면 트랜잭션은 99.9%의 신뢰도로 확인(confirm)되었다 판단할 수 있습니다.

There are some extraordinary cases where a software bug, Internet congestion, or a malicious block producer will create two or more forks. For absolute certainty that a transaction is irreversible, a node may choose to wait for confirmation by 15 out of the 21 block producers.  Based on a typical configuration of the EOS.IO software, this will take an average of 45 seconds under normal circumstances. By default all nodes will consider a block confirmed by 15 of 21 producers irreversible and will not switch to a fork that excludes such a block regardless of length.

소프트웨어 버그, 인터넷 속도 저하, 비정상적 블록 생산자와 같은 특수한 상황에서 두 개 혹은 그 이상의 포크가 생성될 수 있습니다. 트랜잭션의 바뀌지 않음(irreversible)을 확정하기 위해서, 노드는 21명의 블록 생산자 중 15명의 확인(confirmation)을 기다릴 수 있습니다. EOS.IO 소프트웨어의 기본 설정에 따르면 보통 상황에서 45초의 시간이 소요됩니다. 기본적으로 모든 노드는 21명 중 15명이 확인할 경우 해당 블록이 바뀌지 않음을 확정하고 블록의 길이와 상관없이 다른 포크로 전환하지 않습니다.

It is possible for a node to warn users that there is a high probability that they are on a minority fork within 9 seconds of the start of a fork. After 2 consecutive missed blocks there is a 95% probability a node is on a minority fork.  With 3 consecutive missed blocks there is a 99% certainty of being on a minority fork.  It is possible to generate a robust predictive model that will utilize information about which nodes missed, recent participation rates, and other factors to quickly warn operators that something is wrong.

노드는 포크가 분기된 후 9초 이내에 속한 노드가 소수 포크(minority fork)에 속해있음을 높은 확률로 사용자에게 알려줄 수 있습니다. 노드가 속한 블록체인에 2개의 블록이 연속적으로 추가되지 않을 경우 95% 확률로 소수 포크에 속하게 됩니다. 3번 연속으로 추가되지 않을 경우 99%의 소수 포크 확률을 가집니다. 블록이 Qkwls 노드, 최근 참여 비율 등의 정보를 토대로 관리자에게 잘못된 상황에 대한 경보를 신속하게 제공하는 안정적인 예측 모형을 만들 수 있습니다.

The response to such a warning depends entirely upon the nature of the business transactions, but the simplest response is to wait for 15/21 confirmations until the warning stops.

경보에 대한 대응은 비즈니스 트랜잭션의 성격에 따라 다르며, 가장 간단한 대응 방법은 15/21 확인이 이루어져 경보가 끝나는 시점까지 기다리는 것입니다.

## Transaction as Proof of Stake (TaPoS)

## 트랜잭션 기반 지분 증명 (Transaction as Proof of Stake, TaPoS)

The EOS.IO software requires every transaction to include the hash of a recent block header. This hash serves two purposes:

EOS.IO 소프트웨어는 모든 트랜잭션이 최근 블록 헤더의 해쉬값을 포함하도록 요구합니다. 해쉬 값은 두 가지 용도로 사용됩니다.

1. prevents a replay of a transaction on forks that do not include the referenced block; and

1. 참조 블록(referenced block)이 포함되지 않은 포크에서 트랜잭션이 재실행 되는 것을 방지합니다.

2. signals the network that a particular user and their stake are on a specific fork.

2. 특정 사용자가 가진 자산이 어떤 포크에서 존재하는지 네트워크에 알려줍니다.

Over time all users end up directly confirming the blockchain which makes it difficult to forge counterfeit chains as the counterfeit would not be able to migrate transactions from the legitimate chain.  

시간이 지날수록 모든 사용자는 직접 블록체인을 확인(confirm)하게 되며, 합법적 체인의 거래를 위조 체인으로 옮길 수 없으므로 위조 체인을 만드는 것은 어렵게 됩니다.

# Accounts

# 계정 (Accounts)

The EOS.IO software permits all accounts to be referenced by a unique human readable name of 2 to 32 characters in length. The name is chosen by the creator of the account.  All accounts must be funded with the minimal account balance at the time they are created to cover the cost of storing account data.  Account names also support namespaces such that the owner of account @domain is the only one who can create the account @user.domain.

EOS.IO 소프트웨어는 모든 계정이 2~32글자의 읽을 수 있는 고유 이름으로 참조되도록 합니다. 계정 이름은 생성자가 선택합니다. 모든 계정은 생성되는 시점에 계정 정보를 담는 저장 비용을 초과하는 잔액을 보유해야 합니다. 계정 이름은 네임스페이스를 지원합니다. 예를 들어, @domain 계정의 소유자만이 @user.domain을 생성할 수 있도록 합니다.

In a decentralized context, application developers will pay the nominal cost of account creation to sign up a new user. Traditional businesses already spend significant sums of money per customer they acquire in the form of advertizing, free services, etc. The cost of funding a new blockchain account should be insignificant in comparison. Fortunately, there is no need to create accounts for users already signed up by another application.   

탈중앙화 환경에서, 애플리케이션 개발자는 새로운 사용자가 가입하여 계정을 생성하는 최소한의 비용을 부담해야 합니다. 기존의 사업은 이미 광고, 무료 서비스 등 다양한 형태로 고객들에게 상당한 금액을 지급하고 있습니다. 이와 비교할 때 새로운 블록체인 계정에 부담하는 비용은 상대적으로 미미할 것입니다. 이미 다른 애플리케이션에 가입한 사용자의 계정을 중복하여 가입할 필요는 없습니다.

## Messages & Handlers

## 메시지와 처리기 (Messages & Handlers)

Each account can send structured messages to other accounts and may define scripts to handle messages when they are received.  The EOS.IO software gives each account its own private database which can only be accessed by its own message handlers. Message handling scripts can also send messages to other accounts. The combination of messages and automated message handlers is how EOS.IO defines smart contracts.

한 계정에서 다른 계정으로 구조화된 메시지(structured message)를 전송할 수 있으며, 이를 송신하였을 때 처리하는 스크립트(script)를 정의할 수 있습니다. EOS.IO 소프트웨어는 각각의 계정이 독립된 프라이빗 데이터베이스를 가지도록 하며, 자신의 메시지 처리기만이 접근하도록 허용합니다. 메시지 처리 스크립트에서 다른 계정으로 메시지를 전송할 수도 있습니다. EOS.IO는 스마트 컨트렉트(smart contract)를 메시지와 자동화된 메시지 처리기의 조합으로 정의합니다.

## Role Based Permission Management

## 역할 기반 권한 관리 (Role Based Permission Management)

Permission management involves determining whether or not a message is properly authorized. The simplest form of permission management is checking that a transaction has the required signatures, but this implies that required signatures are already known. Generally authority is bound to individuals or groups of individuals and is often compartmentalized. The EOS.IO software provides a declarative permission management system that gives accounts fine grained and high level control over who can do what and when.

권한 관리는 메시지가 정상적으로 인증(authorized)되었는지 결정하는 것을 포함됩니다. 가장 단순한 형태의 권한 관리는 트랜잭션의 서명 여부를 검사하는 것이나, 이는 이미 필요한 서명을 알고 있을 때 가능합니다. 일반적으로 권한은 사용자와 그룹에 부여되며 이는 종종 구분됩니다. EOS.IO 소프트웨어는 계정별로 누가, 언제, 어떤 작업을 할 수 있는지에 대하여 세부적으로 제어하는 선언적 권한 관리 시스템을 제공합니다.

It is critical that authentication and permission management be standardized and separate from the business logic of the application. This enables tools to be developed to manage permissions in a general purpose manner and also provide significant opportunities for performance optimization.  

인증과 권한 관리를 표준화하고 애플리케이션의 비즈니스 로직과 분리하는 것이 중요합니다. 이것은 권한 관리가 범용적인 관점에서 이루어지도록 하는 도구를 개발할 수 있게 하며, 또한 성능 최적화의 큰 가능성이 열리게 합니다.

Every account may be controlled by any weighted combination of other accounts and private keys. This creates a hierarchical authority structure that reflects how permissions are organized in reality, and makes multi-user control over funds easier than ever. Multi-user control is the single biggest contributor to security, and, when used properly, it can greatly eliminate the risk of theft due to hacking.

모든 계정은 다른 계정들(other accounts)과 개인키들(private keys)의 가중치 조합(weighted combination)으로 제어될 수 있습니다. 이를 통해 현실의 권한 구성 방식과 유사한 위계적 인증 구조(hierarchical authority structure)를 생성할 수 있으며, 또한 기금(funds)에 대한 다중 사용자 제어(multi-user control)를 보다 쉽게 하도록 합니다. 다중 사용자 제어는 보안 관점에서 큰 의의가 있으며, 이를 올바르게 사용할 경우 해킹으로 인한 도난의 위험을 큰폭으로 감소시킬 수 있습니다.

EOS.IO software allows accounts can define what combination of keys and/or other accounts can send a particular message type to another account.  For example, it is possible to have one key for a user's social media account and another for access to the exchange.  It is even possible to give other accounts permission to act on behalf of a user's account without  assigning them keys.  

EOS.IO 소프트웨어는 타 계정으로 전송 가능한 메시지 타입을 키와 다른 계정의 자유로운 조합으로 정의할 수 있게 합니다. 예를 들어, 사용자의 소셜 미디어 계정의 키와 별도로 거래 권한의 키를 가질 수 있습니다. 다른 계정에 키를 할당하지 않고도 다른 계정이 사용자의 계정을 대신하여 활동할 수 있도록 할 수도 있습니다.

### Named Permission Levels

### 명명된 권한 수준 (Named Permission Levels)

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Using the EOS.IO software, accounts can define named permission levels each of which can be derived from higher level named permissions. Each named permission level defines an authority; an authority is a threshold multi-signature check consisting of keys and/or named permission levels of other accounts. For example, an account's "Friend" permission level can be set for the account to be controlled equally by any of the account's friends.

EOS.IO 소프트웨어를 이용하여 계정은 상위 명명된 권한 수준들로부터 파생되는 명명된 권한 수준을 정의할 수 있습니다. 각각 명명된 권한 수준은 인증 방식(authority)을 정의합니다. 인증 방식은 다른 계정의 키와 명명된 권한 수준의 자유로운 조합에 대한 역치 다중서명 확인(threshold multi-signature check)입니다. 예를 들어, 한 계정의 "친구" 권한 수준은 해당 계정에 대해 어떠한 친구 계정으로도 동등하게 제어될 수 있게 할 수 있습니다.

Another example is the Steem blockchain which has three hard-coded named permission levels: owner, active, and posting. The posting permission can only perform social actions such as voting and posting, while the active permission can do everything except change the owner.  The owner permission is meant for cold storage and is able to do everything.  EOS.IO generalizes this concept by allowing each account holder to define their own hierarchy as well as the grouping of actions.

다른 예제로서 Steem 블록체인이 있으며, 여기에는 3가지 하드 코딩된 명명된 권한 수준을 가지고 있습니다. 소유자(owner), 활동(active), 포스팅(posting) 입니다. 포스팅 권한은 투표나 글쓰기와 같은 소셜 활동을 할 수 있으며, 활동 권한은 소유자 변경 외 모든 활동이 가능합니다. 소유자(owner) 권한은 콜드 스토리지(cold storage)를 의미하며 모든 활동이 가능합니다. EOS.IO는 steem의 개념을 일반화하여 각각의 계정 소유주가 활동 그룹을 포함하는 독자적인 위계(hierarchy)를 정의할 수 있도록 합니다.

### Named Message Handler Groups

### 명명된 메시지 처리기 그룹 (Named Message Handler Groups)

The EOS.IO software allows each account to organize its own message handlers into named and nested groups. These named message handler groups can be referenced by other accounts when they configure their permission levels.

EOS.IO 소프트웨어는 각각의 계정이 독자적인 메시지 처리기를 명명(named)하고 중첩 그룹화(nested groups)하는 것을 허용합니다. 명명된 메시지 핸들러 그룹(names message handler group)들은 권한 수준을 설정할 때 다른 계정으로부터 참조될 수 있습니다.

The highest level message handler group is the account name and the lowest level is the individual message type being received by the account. These groups can be referenced like so:  **@accountname.groupa.subgroupb.MessageType**.

최상위 수준의 메시지 처리기 그룹은 계정 이름이며, 가장 낮은 수준은 특정 계정으로부터 받은 개별적인 메시지 타입입니다. 이러한 그룹들은 다음과 같이 참조될 수 있습니다. **@계정명.그룹A.하위그룹B.메시지타입** (**@accountname.groupa.subgroupb.MessageType**)

Under this model it is possible for an exchange contract to group order creation and canceling separately from deposit and withdraw. This grouping by the exchange contract is a convenience for users of the exchange.

이러한 모형을 이용하여 거래소의 계약을 거래 단위로 그룹화하여 입금과 출금을 별개로 다룰 수 있게 됩니다. 이러한 거래 계약의 그룹화는 교환소의 사용자에게 편의성을 제공합니다.

### Permission Mapping

### 권한 매핑 (Permission Mapping)

EOS.IO software allows each account to define a mapping between a Named Message Handler Group of any account and their own Named Permission Level.  For example, an account holder could map the account holder's social media application to the account holder's "Friend" permission group. With this mapping, any friend could post as the account holder on the account holder's social media.  Even though they would post as the account holder, they would still use their own keys to sign the message. This means it is always possible to identify which friends used the account and in what way.

EOS.IO 소프트웨어는 계정별로 어떠한 계정의 명명된 메시지 처리기 그룹과 보유하고 있는 명명된 관리 수준 간의 매핑을 진행할 수 있도록 합니다. 예를 들어, 계정 소유자는 해당 계정 소유자의 소셜 미디어 애플리케이션을 "친구" 관리 그룹과 매핑할 수 있습니다. 이 매핑을 통해, 어떤 친구들이라도 계정 소유자처럼 계정 소유자의 소셜 미디어에 포스팅할 수 있습니다. 친구들이 계정 소유자처럼 포스팅할지라도 그들이 가진 키로 메시지에 서명합니다. 이는 어떤 친구가 계정을 사용했고 무엇을 하였는지 알 수 있음을 뜻합니다.

### Evaluating Permissions

### 권한 검사 (Evaluating Permissions)

When delivering a message of type "**Action**", from **@alice** to **@bob** the EOS.IO software will first check to see if **@alice** has defined a permission mapping for **@bob.groupa.subgroup.Action**.  If nothing is found then a mapping for **@bob.groupa.subgroup** then **@bob.groupa**, and lastly **@bob** will be checked. If no further match is found, then the assumed mapping will be to the named permission group **@alice.active**.  

**@앨리스**가 **@밥**에게 "**액션**" 타입의 메시지를 보낸다 가정해봅시다. EOS.IO 소프트웨어는 먼저 **@앨리스**가 **@밥.그룹A.하위그룹.액션** 처리기에 대한 권한을 매핑하였는지 확인합니다. 만약 매핑되어 있지 않다면 매핑이 발견될 때 까지 **@밥.그룹A.하위그룹**, **@밥.그룹A**, **@밥**의 순서로 검사합니다. 만약 메시지 처리기에 대한 매핑이 없으면 **@앨리스.활동(active)** 명명된 권한 그룹 매핑을 가정합니다.

Once a mapping is identified then signing authority is validated using the threshold multi-signature process and the authority associated with the named permission. If that fails, then it traverses up to the parent permission and ultimately to the owner permission, **@alice.owner**.  

만약 매핑이 확인이 되면 역치 다중서명 절차와 명명된 권한에 대한 인증을 통해 서명된 인증에 대한 유효성을 검증합니다. 실패할 경우 상위 권한으로 검사를 수행하며 최종적으로 소유자 권한인 **@앨리스.소유자(owner)**까지 검사를 진행합니다.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Default Permission Groups

#### 기본 권한 그룹( Default Permission Groups)

The technology also allows all accounts have an "owner" group which can do everything, and an "active" group which can do everything except change the owner group.  All other permission groups are derived from "active".  

모든 계정은 제한이 없는 "소유자(owner)" 권한과 소유자 변경 외 모든 것이 가능한 "활동(active)" 권한을 가집니다. 그외 다른 권한은 "활동(active)"으로부터 파생됩니다.

#### Parallel Evaluation of Permissions

#### 권한 검사의 병렬화 (Parallel Evaluation of Permissions)

The permission evaluation process is "read-only" and changes to permissions made by transactions do not take effect until the end of a block. This means that all keys and permission evaluation for all transactions can be executed in parallel. Furthermore, this means that a rapid validation of permission is possible without starting the costly application logic that would have to be rolled back. Lastly, it means that transaction permissions can be evaluated as pending transactions are received and do not need to be re-evaluated as they are applied.

권한 검사 절차는 "읽기 작업만" 필요로 하며 트랜잭션으로 인한 권한의 변경은 블록이 종료될 때까지는 어떠한 영향도 발휘하지 못합니다. 이것은 모든 트랜잭션의 모든 키와 권한 검사가 병렬적으로 진행될 수 있음을 뜻합니다. 더 나아가, 롤백을 고려해야 할 수도 있어 처리 비용이 많이 발생하는 애플리케이션 로직이 없는 신속한 권한 검사의 가능함도 뜻합니다. 마지막으로, 이는 보류 중인 트랜잭션이 수신될 때 트랜잭션 권한을 평가할 수 있으며, 승인된 트랜잭션은 다시 검사할 필요가 없습니다.

All things considered, permission verification represents a significant percentage of the computation required to validate transactions. Making this a read-only and trivially parallelizable process enables a dramatic increase in performance.

전체적인 관점에서, 권한 검사는 트랜잭션 유효성 검사에서 많은 연산 비중을 가집니다. 그러므로 권한 검사를 읽기 연산만으로 구성하여 병렬화하면 전체 성능의 큰 향상을 얻게 됩니다.

When replaying the blockchain to regenerate the deterministic state from the log of messages there is no need to evaluate the permissions again. The fact that a transaction is included in a known good block is sufficient to skip this step. This dramatically reduces the computational load associated with replaying an ever growing blockchain.

메시지 로그(message log)로부터 블록체인의 결정된 상태(deterministic state)로 재구축(replay)하는 과정에서 권한 검사를 다시 수행할 필요는 없습니다. 정상적인 블록에 트랜잭션이 포함되어 있으므로 이 과정을 넘길 수 있습니다. 점진적으로 과대해지는 블록체인의 재구축에 드는 계산 비용을 큰 폭으로 감소시킬 수 있습니다.

## Messages with Mandatory Delay

## 메시지의 필수 지연 시간 (Messages with Mandatory Delay)

Time is a critical component of security. In most cases, it is not possible to know if a private key has been stolen until it has been used. Time based security is even more critical when people have applications that require keys be kept on computers connected to the internet for daily use.  The EOS.IO software enables application developers to indicate that certain messages must wait a minimum period of time after being included in a block before they can be applied. During this time they can be canceled.  

시간은 보안의 핵심 요소입니다. 대부분의 경우, 개인 키(private key)가 도난당한 경우 사용되기 전까지 알기 어렵습니다. 인터넷에 연결된 컴퓨터에 보관되는 키를 사용하는 애플리케이션을 일상적으로 사용하는 사람들에게 시간 기반의 보안은 더욱더 중요합니다. EOS.IO 소프트웨어는 애플리케이션 개발자가 특정 메시지가 블록에 포함되기 시작하고 적용되기 이전에 지정한 시간 만큼을 기다리도록 표시할 수 있게 합니다. 지정한 시간 내에서 메시지는 취소 가능합니다.

Users can then receive notice via email or text message when one of these messages is broadcast. If they did not authorize it, then they can use the account recovery process to recover their account and retract the message.

사용자는 이메일이나 단문으로 메시지가 전송되었음을 안내받을 수 있습니다. 만약 본인이 한 것이 아니라면, 계정 복구 절차를 통해 계정 복구와 메시지 철회를 할 수 있습니다.

The required delay depends upon how sensitive an operation is. Paying for a coffee can have no delay and be irreversible in seconds, while buying a house may require a 72 hour clearing period. Transferring an entire account to new control may take up to 30 days. The exact delays chosen are up to application developers and users.

요구되는 지연 시간은 작업의 중요도에 따라 다릅니다. 커피 한잔을 구매하는 것은 지연시간을 갖지 않아 몇 초 내로 취소 불가 상태가 되며, 집을 사는 문제라면 72시간의 거래 완료 조정 기간을 둘 수 있습니다. 전체 계정을 새로운 환경으로 전환하는 것은 30일의 유예기간을 둘 수도 있습니다. 실제 지연 시간은 애플리케이션 개발자와 사용자가 정하게 될 것입니다.

## Recovery from Stolen Keys

## 키 도난 상태에서의 복구 (Recovery from Stolen Keys)

The EOS.IO software provides users a way to restore control of their account when their keys are stolen. An account owner can use any owner key that was active in the last 30 days along with approval from their designated account recovery partner to reset the owner key on their account. The account recovery partner cannot reset control of the account without the help of the owner.

EOS.IO 소프트웨어는 사용자의 키가 도난당하였을 때 계정에 대한 복구 작업을 제공합니다. 계정 소유자는 최근 30일 이내에 사용한 키를 가지고 지정된 계정 복구 협력자와 협력하여 계정을 재설정할 수 있습니다. 계정 복구 협력자는 소유자의 허가 없이 계정에 작업할 수 없습니다.

There is nothing for the hacker to gain by attempting to go through the recovery process because they already "control" the account. Furthermore, if they did go through the process, the recovery partner would likely demand identification and multi-factor authentication (phone and email).  This would likely compromise the hacker or gain the hacker nothing in the process.

키를 가진 해커는 이미 계정을 제어할 수 있으므로 복구 프로세스를 시도하여 얻을 수 있는 것은 없습니다. 복구 프로세스에 진입하여도 복구 협력자는 추가적인 신분 증명이나 2채널 인증(핸드폰이나 이메일)을 요구할 것입니다. 이 과정에서 해커의 정체가 노출되거나 아무것도 얻지 않을 수 있습니다.

This process is also very different from a simple multi-signature arrangement. With a multi-signature transaction, there is another company that is party to every transaction that is executed, but with the recovery process the agent is only a party to the recovery process and has no power over the day-to-day transactions. This dramatically reduces costs and legal liabilities for everyone involved.

이 과정은 단순한 다중서명 합의(multi-signature arrangement)와 매우 다릅니다. 다중서명 트랜잭션의 경우 모든 트랜잭션에 관여하는 별도의 회사가 존재하는 것입니다. 복구 처리 과정을 이용하면 복구 협력자는 복구과정에만 관여하며 트랜잭션에 관해서는 어떠한 영향력도 행사하지 않습니다. 이로 인해 참여자들에 대한 비용과 법적 책임을 큰폭으로 감소시킬 수 있습니다.

# Deterministic Parallel Execution of Applications

# 애플리케이션의 결정론적 병렬 실행 (Deterministic Parallel Execution of Applications)

Blockchain consensus depends upon deterministic (reproducible) behavior. This means all parallel execution must be free from the use of mutexes or other locking primitives.  Without locks there must be some way to guarantee that all accounts can only read and write their own private database.  It also means that each account processes messages sequentially and that parallelism will be at the account level.  

블록체인 합의(consensus)는 재현 가능한 결정론적 행위(deterministic behavior)에 달려있습니다. 이는 모든 병렬 실행은 뮤텍스(mutex)나 다른 기초 라킹 연산(locking primitive) 없이 수행되어야 함을 뜻합니다. 락(lock)이 없다면 다른 방식으로 모든 계정이 보유한 프라이빗 데이터베이스에만 읽기/쓰기 연산할 수 있도록 보장하여야 합니다. 또한 각각의 계정안의 메시지들을 순차적으로 처리하며, 연산의 병렬성은 계정 단위에서 수행됨을 뜻합니다.

Using the EOS.IO software, it is the job of the block producer to organize message delivery into independent threads so that they can be evaluated in parallel.  The state of each account depends only upon the messages delivered to it. The schedule is the output of a block producer and will be deterministically executed, but the process for generating the schedule need not be deterministic. This means that block producers can utilize parallel algorithms to schedule transactions.

EOS.IO 소프트웨어를 사용할 때, 메시지를 개별적인 쓰레드로 구성하여 병렬 평가되도록 하는 것은 블록 생산자가 수행하는 일입니다. 각각의 계정의 상태(state)는 전달받은 메시지에 달려있습니다. 블록 생산자의 결과물로 실행 스케쥴이 나오게 되며, 이는 결정론적으로 실행됩니다. 다만, 스케쥴을 만드는 과정까지 결정론적일 필요는 없습니다. 이는 블록 생산자가 트랜잭션을 스케쥴링 할 때 병렬 알고리즘을 활용할 수 있음을 뜻합니다.

Part of parallel execution means that when a script generates a new message it does not get delivered immediately, instead it is scheduled to be delivered in the next cycle. The reason it cannot be delivered immediately is because the receiver may be actively modifying its own state in another thread.

스크립트가 새로운 메시지를 만들 때, 바로 전달되지 않고 다음 사이클(cycle)에 전달되도록 스케쥴을 구성하는 것을 부분적 병렬 실행이라 합니다. 메시지를 바로 전달할 수 없는 이유는 수신자(receiver)가 다른 쓰레드로 인해 상태가 변경되고 있을 수 있기 때문입니다.

## Minimizing Communication Latency

## 통신 지연 최소화 (Minimizing Communication Latency)

Latency is the time it takes for one account to send a message to another account and then receive a response.  The goal is to enable two accounts to exchange messages back and forth within a single block without having to wait 3 seconds between each message. To enable this, the EOS.IO software divides each block into cycles.  Each cycle is divided into threads and each thread contains a list of transactions.  Each transaction contains a set of messages to be delivered. This structure can be visualized as a tree where alternating layers are processed sequentially and in parallel.

지연 시간은 한 계정에서 다른 계정으로 메시지를 보내고 응답을 받기까지의 시간입니다. 기술적 목표는 두 계정 사이의 메시지 교환이 단일 블록에서 이루어지며 메시지 간 간격이 3초 이내에 들어가게 하는 것입니다. 목표를 달성하기 위해 EOS.IO 소프트웨어는 블록을 사이클 단위로 나눕니다. 각각의 트랜잭션은 전달하는 메시지의 집합으로 구성됩니다. 이러한 구조는 트리(tree) 형태로 시각화 가능하며, 레이어 단위로 순차(sequentially) 처리되거나 병렬(parallel) 처리됩니다.

```
      Block

        Cycles (sequential)

          Threads (parallel)

            Transactions (sequential)

              Messages (sequential)

                Receiver and Notified Accounts (parallel)
```

```
      블록(Block)

        사이클(Cycles) (순차 처리)

          쓰레드(Threads) (병렬 처리)

            트랜잭션(Transactions) (순차 처리)

              메시지(Messages) (순차 처리)

                수신자 및 계정 알림(Receiver and Notified Accounts) (병렬 처리)
```

Transactions generated in one cycle can be delivered in any subsequent cycle or block. Block producers will keep adding cycles to a block until the maximum wall clock time has passed or there are no new generated transactions to deliver.

하나의 사이클에서 생성된 트랜잭션들은 이후 사이클 혹은 블록에서 전달됩니다. 블록 생산자는 블록에 지정된 시간(3초)이 지나거나 더 전달할 트랜잭션이 없을 때까지 사이클을 계속 추가합니다.

It is possible to use static analysis of a block to verify that within a given cycle no two threads contain transactions that modify the same account. So long as that invariant is maintained a block can be processed by running all threads in parallel.

정적 분석(static analysis)으로 한 블록에서 동일 사이클에 같은 계정을 수정하는 2개 이상의 쓰레드를 가진 트랜잭션이 없는지 알 수 있습니다. 없으면, 하나의 블록은 여러 개의 쓰레드로 병렬 처리할 수 있습니다.

## Read-Only Message Handlers

## 읽기 전용 메시지 처리기 (Read-Only Message Handlers)

Some accounts may be able to process a message on a pass/fail basis without modifying their internal state. If this is the case then these handlers can be executed in parallel so long as only read-only message handlers for a particular account are included in one or more threads within a particular cycle.

특정 계정의 메시지는 내부 상태의 수정이 아닌 통과/실패(pass/fail) 기반으로 처리할 때도 있습니다. 이러면 특정 사이클에 하나 이상의 쓰레드가 포함될 경우 해당 작업을 수행하는 메시지 처리기는 병렬 처리가 가능합니다.

## Atomic Transactions with Multiple Accounts

## 다중 계정의 원자적 트랜잭션 (Atomic Transactions with Multiple Accounts)

Sometimes it is desirable to ensure that messages are delivered to and accepted by multiple accounts atomically. In this case both messages are placed in one transaction and both accounts will be assigned the same thread and the messages applied sequentially. This situation is not ideal for performance and when it comes to "billing" users for usage, they will get billed by the number of unique accounts referenced by a transaction.

종종 다수의 계정에 메시지의 전달 및 적용에 대한 원자성(atomicity)을 보장해야 합니다. 이 경우, 두 메시지는 하나의 트랜잭션에 위치하고, 두 계정은 같은 쓰레드에 할당되며, 메시지는 순차적으로 적용됩니다. 이 방식은 성능 면에서 이상적이지 않습니다. 사용량에 대한 "청구(billing)"를 수행할 때 트랜잭션에 참고된 계정의 수 만큼 비용이 청구됩니다.

For performance and cost reasons it is best to minimize atomic operations involving two or more heavily utilized accounts.  

성능과 비용 측면에서 2개 이상의 계정이 참여하는 원자적 트랜잭션을 줄이는 것이 좋습니다.

## Partial Evaluation of Blockchain State

## 블록체인 상태의 부분 검사 (Partial Evaluation of Blockchain State)

Scaling blockchain technology necessitates that components are modular. Everyone should not have to run everything, especially if they only need to use a small subset of the applications.

블록체인 기술의 확장성을 보장하려면 구성 요소는 모듈화되어야 합니다. 애플리케이션의 일부만 사용할 때 전체를 다 구동할 필요는 없습니다.

An exchange application developer runs full nodes for the purpose of displaying the exchange state to its users. This exchange application has no need for the state associated with social media applications. EOS.IO software allows any full node to pick any subset of applications to run. Messages delivered to other applications are safely ignored because an application's state is derived entirely from the messages that are delivered to it.  

거래소 애플리케이션의 개발자는 거래 상태를 사용자들에게 보여주기 위해 풀 노드(full node)를 구동하여야 합니다. 이러한 거래소 애플리케이션은 소셜 미디어 애플리케이션과 연동하여 동작할 필요가 없습니다. EOS.IO 소프트웨어는 풀 노드가 애플리케이션 중 일부를 한정 지어 구동할 수 있도록 합니다. 전달받은 메시지를 통해 애플리케이션의 상태가 전이되기 때문에, 다른 애플리케이션에서 이루어지는 메시지 전송은 안전하게 무시됩니다. 

This has some significant implications on communication with other accounts. Most significantly it cannot be assumed that the state of the other account is accessible on the same machine. It also means that while it is tempting to enable "locks" that allow one account to synchronously call another account, this design pattern breaks down if the other account is not resident in memory.

이것은 다른 계정과의 통신에 중요한 의미를 가집니다. 특히 다른 계정의 상태를 동일 기계(machine)로 접근할 수 있다고 가정하면 안 됩니다. 또한, 다른 계정과의 동기화 콜(call)을 호출하도록 로크(lock)를 사용할 때, 다른 계정이 메모리에 상주하지 않으면 이 디자인 패턴은 손상됩니다.

All state communication among accounts must be passed via messages included in the blockchain.

계정 간 상태 통신(state communication)은 블록체인의 메시지로 전달되어야 합니다.

## Subjective Best Effort Scheduling

## 주관적 최선 스케쥴링 (Subjective Best Effort Scheduling)

The EOS.IO software cannot obligate block producers to deliver any message to any other account. Each block producer makes their own subjective measurement of the computational complexity and time required to process a transaction. This applies whether a transaction is generated by a user or automatically by a script.

EOS.IO 소프트웨어는 블록 생산자가 어떤 계정으로 어떻게 메시지를 보낼지에 대하여 강제할 수 없습니다. 각각의 블록 생산자는 계산 복잡도와 트랜잭션을 처리하기 위한 요구 시간에 대한 주관적인 측정을 수행합니다. 이것은 트랜잭션이 사용자에 의해 생성되거나 스크립트에 의해 자동으로 생성될 때 모두 적용됩니다.

The EOS.IO software provides that at a network level all transactions are billed a fixed computational bandwidth cost regardless of whether it took .01ms or a full 10 ms to execute it. However, each individual block producer using the software may calculate resource usage using their own algorithm and measurements. When a block producer concludes that a transaction or account has consumed a disproportionate amount of the computational capacity they simply reject the transaction when producing their own block; however, they will still process the transaction if other block producers consider it valid.

EOS.IO 소프트웨어는 네트워크 관점에서 모든 트랜잭션에 대해 0.01ms가 걸리건 10ms가 걸리건 상관없이 같은 연산 대역폭(compudational bandwidth) 비용을 청구합니다. 하지만 블록 생산자들은 개별적인 측정 알고리즘을 이용하여 리소스 사용 비용을 계산할 수 있습니다. 블록 생산자가 한 트랜잭션 혹은 계정이 과도한 양의 연산 자원을 사용한다고 판단이 되면 해당 트랜잭션을 본인이 생산하는 블록에 추가하는 것을 거부할 수 있습니다. 물론 이 경우에도 다른 블록 생산자는 그 트랜잭션이 적합하다고 판단하여 처리할 수 있습니다.

In general so long as even 1 block producer considers a transaction as valid and under the resource usage limits then all other block producers will also accept it, but it may take up to 1 minute for the transaction to find that producer.

일반적으로 한명 의 블록 생산자라도 트랜잭션이 리소스 사용 제한을 넘지 않아 적합하다고 간주하면 다른 블록 생산자도 승인하게 됩니다. 그러나, 이 경우 트랜잭션이 받아주는 블록 생산자를 찾기까지 1분 가까운 시간이 소요될 수 있습니다.

In some cases a producer may create a block that includes transactions that are an order of magnitude outside of acceptable ranges. In this case the next block producer may opt to reject the block and the tie will be broken by the third producer. This is no different than what would happen if a large block caused network propagation delays. The community would notice a pattern of abuse and eventually remove votes from the rogue producer.

간혹 블록 생산자가 허용 가능한 범위를 몇 배나 넘어가는 트랜잭션을 블록에 포함 시킬 수도 있습니다. 이 경우 다음 블록 생산자는 그 블록을 아예 거절해버릴 수 있으며, 승인과 거절이 동률인 상태는 다음 블록 생산자에 의해 판결됩니다. 이는 거대 블록으로 인해 네트워크 전파 지연이 발생하는 경우와 차이가 없습니다. 커뮤니티는 이상 패턴을 파악하고 이러한 불량 생산자에게 표를 주지 말아야 할 것입니다.

This subjective evaluation of computational cost frees the blockchain from having to precisely and deterministically measure how long something takes to run. With this design there is no need to precisely count instructions which dramatically increases opportunities for optimization without breaking consensus.

이러한 블록 생산자의 주관적 평가가 있기에 블록체인은 정확하고 명확한 실행 시간 계산의 측정을 하지 않아도 됩니다. 이러한 디자인은 정확한 계수 명령(count instruction)을 요구하지 않으며, 이로 인해 합의를 해치지 않는 최적화 기회가 열리게 됩니다.

# Token Model and Resource Usage

# 토큰 모델과 리소스 사용 (Token Model and Resource Usage)

All blockchains are resource constrained and require a system to prevent abuse. With the EOS.IO software, there are three broad classes of resources that are consumed by applications:

모든 블록체인의 리소스는 제한적이며 부당 사용을 막는 장치가 필요합니다. EOS.IO 소프트웨어는 리소스를 크게 세 가지 분류로 나눕니다.

1. Bandwidth and Log Storage (Disk);
2. Computation and Computational Backlog (CPU); and
3. State Storage (RAM).

1. 대역폭과 로그 저장소 (디스크)
2. 연산과 연산 로그 (CPU)
3. 상태 저장소 (램)

Bandwidth and computation have two components, instantaneous usage and long-term usage. A blockchain maintains a log of all messages and this log is   and downloaded by all full nodes. With the log of messages it is possible to reconstruct the state of all applications.

대역폭과 연산은 즉시 사용과 장기 사용의 2개의 구성요소가 있습니다. 블록체인은 모든 메시지의 로그를 관리합니다. 로그는 저장되며 풀 노드(full node)에 다운로드 됩니다. 메시지의 로그를 통해 모든 애플리케이션의 상태를 재구축할 수 있습니다.

The computational debt is calculations that must be performed to regenerate state from the message log. If the computational debt grows too large then it becomes necessary to take snapshots of the blockchain's state and discard the blockchain's history. If computational debt grows too quickly then it may take 6 months to replay 1 year worth of transactions. It is critical, therefore, that the computational debt be carefully managed.

메시지 로그로부터 상태를 재구축하기 위해 수행되는 계산을 연산 부채(compudational debt)라 합니다. 만약에 연산 부채가 과다하게 증가하면 블록체인의 상태의 스냅샷(snapshot)을 저장하고, 과거 이력을 제거할 필요성이 생깁니다. 만약에 연산 부채가 너무 빠르게 증가하면 때론 1년의 트랜잭션을 재현하기 위해 6개월의 시간이 필요할 수도 있습니다. 이는 매우 치명적이므로 연산 부채는 주의 깊게 관리되어야 합니다.

Blockchain state storage is information that is accessible from application logic. It includes information such as order books and account balances. If the state is never read by the application then it should not be stored. For example, blog post content and comments are not read by application logic so they should not be stored in the blockchain's state.  Meanwhile the existence of a post/comment, the number of votes, and other properties do get stored as part of the blockchain's state.

블록체인 상태 저장소는 애플리케이션 로직이 접근할 수 있는 정보입니다. 이는 계정 잔액과 거래 내용 등의 정보를 담고 있습니다. 만약에 애플리케이션이 특정 상태를 읽는 일이 없다면 굳이 저장할 필요가 없습니다. 예를 들어, 블로그 작성 글의 내용과 댓글 내용은 애플리케이션 로직에 의해 읽히지 않으므로 블록체인 상태에 저장할 필요가 없습니다. 반면에 글/댓글의 존재 여부와 투표 횟수 등의 속성값은 블록체인 상태에 기록되어야 합니다.

Block producers publish their available capacity for bandwidth, computation, and state. The EOS.IO software allows each account to consume a percentage of the available capacity proportional to the amount of tokens held in a 3-day staking contract. For example, if a blockchain based on the EOS.IO software is launched and if an account holds 1% of the total tokens distributable pursuant to that blockchain, then that account has the potential to utilize 1% of the state storage capacity.

블록 생산자는 그들이 가용 가능한 대역폭, 연산 능력, 상태 허용량을 알려주어야 합니다. EOS.IO 소프트웨어는 각각의 계정이 3일간 누적한 계약의 토큰 양에 비례하여 자원을 사용하게 허용합니다. 예를 들어, EOS.IO 기반의 블록체인이 출시한 후 한 계정이 배포 가능한 전체 토큰의 1%를 가지고 있으면 해당 계정은 전체 상태 저장소의 1%를 사용할 수 있습니다.

Using the EOS.IO software, bandwidth and computational capacity are allocated on a fractional reserve basis because they are transient (unused capacity cannot be saved for future use). The algorithm used by EOS.IO is similar to the algorithm used by Steem to rate-limit bandwidth usage.

EOS.IO 소프트웨어를 에서 대역폭과 연산 허용량은 일시적(사용하지 못한 리소스를 나중에 다시 쓸 수 없음)이므로 예비 예약 기준으로 할당됩니다. EOS.IO의 알고리즘은 스팀에서 대역폭 사용량의 속도를 제어하는 알고리즘과 유사합니다.

## Objective and Subjective Measurements

## 객관적 측정과 주관적 측정 (Objective and Subjective Measurements)

As discussed earlier, instrumenting computational usage has a significant impact on performance and optimization; therefore, all resource usage constraints are ultimately subjective and enforcement is done by block producers according to their individual algorithms and estimates.

연산 사용량의 계측은 성능과 최적화에 큰 영향을 미칩니다. 그러므로 모든 리스스 사용 제한은 궁극적으로 블록 생산자의 개별적인 측정 방식과 알고리즘에 의하여 주관적으로 이루어져야 합니다.

That said, there are certain things that are trivial to measure objectively. The number of messages delivered and the size of the data stored in the internal database are cheap to measure objectively. The EOS.IO software enables block producers to apply the same algorithm over these objective measures but may choose to apply stricter subjective algorithms over subjective measurements.  

객관적으로 측정 가능한 것들도 있습니다. 메시지 전달 수와 내부 데이터베이스에 저장하는 데이터의 양은 객관적으로 측정할 수 있습니다. EOS.IO 소프트웨어는 블록 생산자가 객관적 측정을 위한 같은 알고리즘을 적용할 수 있게 합니다. 물론 주관적 측정 방식을 위한 주관적 알고리즘만 적용하게 할 수도 있습니다.

## Receiver Pays

## 수취인 부담 (Receiver Pays)

Traditionally, it is the business that pays for office space, computational power, and other costs required to run the business. The customer buys specific products from the business and the revenue from those product sales is used to cover the business costs of operation. Similarly, no website obligates its visitors to make micropayments for visiting its website to cover hosting costs. Therefore, decentralized applications should not force its customers to pay the blockchain directly for the use of the blockchain.

전통적으로, 사업을 하기 위해서는 오피스 공간, 연산 능력, 그 외의 요소들을 구매해야 하며 여기에서 비용이 발생합니다. 고객이 특정 물품을 구매할 때, 벌어들인 이익 일부는 이러한 사업 비용을 결재할 때 사용됩니다. 고객이 직접 사업 비용을 내지는 않습니다. 비슷하게 생각해보면, 어떠한 웹사이트도 고객이 소액결제를 할 때 호스팅 비용을 요구하지 않습니다. 그러므로, 탈중앙화 애플리케이션 역시 고객으로 하여금 블록체인을 사용할 때 블록체인 사용료를 요구하여서는 안 됩니다.

The EOS.IO software does not require its users to pay directly to the blockchain for its use and therefore does not constrain or prevent a business from determining its own monetization strategy for its products.

EOS.IO 소프트웨어는 사용자가 블록체인 사용 비용을 직접 비용을 지급할 필요가 없습니다. 그러므로 사업이 상품의 수익 창출 전략을 제한하거나 막지 않습니다.

## Delegating Capacity

## 리소스 허용량 위임 (Delegating Capacity)

If a blockchain is launched using the EOS.IO software and tokens are held by a holder who may not have an immediate need to consume all or part of the available bandwidth, such holder can choose to give or rent the unconsumed bandwidth to others; the block producers running EOS.IO software will recognize this delegation of capacity and allocate bandwidth accordingly.

EOS.IO 소프트웨어를 사용하여 블록체인을 출시하였을 때, 필요한 대역폭보다 많은 토큰을 가지고 있는 소유자를 가정해 봅시다. 해당 소유자는 남은 대역폭을 다른 사람에게 양도하거나 빌려줄 수 있습니다. EOS.IO의 블록 생산자는 이러한 대역폭 허용량의 양도를 인지할 수 있으며, 이에 맞추어 대역폭을 재할당합니다.

## Separating Transaction costs from Token Value

## 토큰의 가치와 트랜잭션 비용의 분리 (Separating Transaction costs from Token Value)

One of the major benefits of the EOS.IO software is that the amount of bandwidth available to an application is entirely independent of any token price. If an application owner holds a relevant number of tokens, then the application can run indefinitely within a fixed state and bandwidth usage. Developers and users are unaffected from any price volatility in the token market and therefore not reliant on a price feed. The EOS.IO software enables block producers to naturally increase bandwidth, computation, and storage available per token independent of the token's value.

EOS.IO 소프트웨어의 큰 장점 중 하나는 애플리케이션에서 필요한 대역폭은 어떠한 토큰 가격과도 독립적이라는 것입니다. 만약에 애플리케이션 소유자가 충분한 양의 토큰을 가지고 있다면, 애플리케이션은 고정된 상태와 대역폭 사용량 내에서 제한 없이 구동됩니다. 개발자와 사용자는 토큰 시장의 가격 변동성에 영향을 받지 않으며, 가격 추이에 연연하지 않게 됩니다. EOS.IO 소프트웨어에서 블록 생산자는 토큰의 가치와 무관하게 자연스럽게 대역폭, 연산 능력, 저장 능력을 향상할 수 있습니다.

The EOS.IO software awards block producers tokens every time they produce the block. The value of the tokens will impact the amount of bandwidth, storage, and computation a producer can afford to purchase; this model naturally leverages rising token values to increase network performance.

EOS.IO 소프트웨어는 블록 생산자가 블록을 생성할 때마다 보상으로 토큰을 제공합니다. 토큰의 가치는 블록 생산자가 구매할 수 있는 대역폭, 저장소, 연산장치에 영향을 줍니다. 이러한 모델은 토큰 가치의 상승을 이용하여 네트워크 성능을 향상합니다.

## State Storage Costs

## 상태 저장 비용 (State Storage Costs)

While bandwidth and computation can be delegated, storage of application state will require an application developer to hold tokens until that state is deleted. If state is never deleted then the tokens are effectively removed from circulation.

대역폭과 연산 능력은 위임할 수 있지만, 애플리케이션 상태가 제거되기 전까지 애플리케이션 상태 저장소 유지를 위한 토큰을 보유해야 합니다. 만약에 상태가 영원히 유지된다면, 해당 토큰은 순환과정을 통해 효과적으로 제거됩니다.

Every user account requires a certain amount of storage; therefore, every account must maintain a minimum balance. As storage capacity of the network increases this minimum required balance will fall.

모든 사용자 계정은 어느 정도의 저장 공간이 필요합니다. 그러므로 모든 계정은 최소한의 잔액을 가지고 있어야 합니다. 네트워크의 저장 능력이 향상되면 최소 잔액은 줄어들 것입니다.

## Block Rewards

## 블록 보상 (Block Rewards)

EOS.IO software awards new tokens to a block producer every time a block is produced.  The number of tokens created is determined by the median of the desired pay published by all block producers. The EOS.IO software may be configured to enforce a cap on producer awards such that the total annual increase in token supply does not exceed 5%.  

EOS.IO 소프트웨어는 블록이 생성될 때마다 블록 생성자에게 보상으로 토큰을 부여합니다. 생성되는 토큰의 양은 블록 생산자들이 제출한 요구 금액의 중앙값으로 결정됩니다. EOS.IO 소프트웨어는 생산자 보상의 총량이 연 토큰 증가분의 5%를 넘지 못하도록 제한을 걸 수 있습니다.

## Community Benefit Applications

## 커뮤니티 혜택 애플리케이션 (Community Benefit Applications)

In addition to electing block producers, based on the EOS.IO software, users can elect 3 community benefit applications also known as smart contracts. These 3 applications will receive tokens of up to a configured percent of the token supply per annum minus the tokens that have been paid to block producers. These smart contracts will receive tokens proportional to the votes each application has received from token holders. The elected applications or smart contracts can be replaced by newly elected applications or smart contracts by token holders.

블록 생산자를 선출함과 더불어, EOS.IO 소프트웨어의 사용자들은 3개의 커뮤니티 애플리케이션(스마트 컨트렉트)을 선정할 수 있습니다. 커뮤니티 애플리케이션은 설정된 연간 토큰 공급량에서 블록 생산자에게 지급한 양을 제외한 토큰을 가지게 됩니다. 이 스마트 컨트렉트들이 받는 토큰의 양은 각 어플리케이션이 토큰 소유자들로부터 받은 투표 수에 비례하여 결정됩니다. 선정된 애플리케이션 혹은 스마트 컨트렉트는 토큰 소유자들의 투표 결과에 따라 바뀔 수 있습니다.

# Governance

# 거버넌스 (Governance)

Governance is the process by which people reach consensus on subjective matters that cannot be captured entirely by software algorithms. The EOS.IO software implements a governance process that efficiently directs the existing influence of block producers. Absent a defined governance process, prior blockchains relied ad hoc, informal, and often controversial governance processes that result in unpredictable outcomes.

거버넌스는 사람들이 소프트웨어 알고리즘으로 결정할 수 없는 주관적 문제에 대하여 합의를 이루는 과정을 뜻합니다. EOS.IO 소프트웨어는 블록 생산자가 가지고 있는 영향력을 효과적으로 행사하는 거버넌스 과정을 구현합니다. 정의된 가버넌스 과정의 부재로 인해 이전의 블록체인들은 즉흥적이고, 비공식적이고, 때로는 논란을 일으키는 거버넌스 과정을 겪었고 예측할 수 없는 결과가 나타났습니다.

The EOS.IO software recognizes that power originates with the token holders who delegate that power to the block producers. The block producers are given limited and checked authority to freeze accounts, update defective applications, and propose hard forking changes to the underlying protocol.    

EOS.IO 소프트웨어는 블록 생산자에게 양도한 토큰 소유자로부터 권력이 발생함을 인지하고 있습니다. 블록 생산자가 가지는 권한은, 계정을 동결하고, 결함이 있는 애플리케이션을 판올림하며, 기본 프로토콜의 변경을 가하는 하드포크를 제안하는 것으로 한정합니다.

Part of the EOS.IO software is the election of block producers. Before any change can be made to the blockchain these block producers must approve it. If the block producers refuse to make changes desired by the token holders then they can be voted out. If the block producers make changes without permission of the token holders then all other non-producing full-node validators (exchanges, etc) will reject the change.

EOS.IO 소프트웨어의 일부는 블록 생산자를 선출하는 것입니다. 블록체인의 모든 변경사항은 블록 생산자에 의해 검수받아야 합니다. 만약에 블록 생산자가 토큰 소유자에 반하는 결정을 내린다면 투표에 의해 지위를 상실할 수 있습니다. 블록 생산자가 토큰 소유자의 허가 없이 변경을 가한다면 모든 비생산 풀 노드 검사자(non-producing full-node validators)(거래소 등)들은 변경을 거부할 수 있습니다.

## Freezing Accounts

## 계정 동결 (Freezing Accounts)

Sometimes a smart contact behaves in an aberrant or unpredictable manner and fails to perform as intended; other times an application or account may discover an exploit that enables it to consume an unreasonable amount of resources. When such issues inevitably occur, the block producers have the power to rectify such situations.

가끔 스마트 컨트렉트는 비정상적이거나 예측하지 못한 방식으로 동작하여 의도했던 기능이 동작하지 않을 수 있습니다. 이외에도 비정상적인 양의 리소스를 소비하는 것을 인지하여 애플리케이션이나 계정에 문제가 있음을 알 수 있습니다. 이러한 문제점이 발생했을 때, 블록 생산자는 이러한 상황을 바로 잡을 방법을 가집니다.

The block producers on all blockchains have the power to select which transactions are included in blocks which gives them the ability to freeze accounts.  EOS.IO software formalizes this authority by subjecting the process of freezing an account to a 17/21 vote of active producers. If the producers abuse the power they can be voted out and an account will be unfrozen.

모든 블록체인의 블록 생산자들은 블록에 포함되는 트랜잭션을 선택하는 능력을 갖추고 있으며, 이를 이용하여 계정을 동결할 수 있습니다. EOS.IO 소프트웨어는 활성화된 블록 생산자들 간의 17/21 투표를 통해 특정 계정에 대한 동결 여부를 의결할 수 있습니다. 만약에 블록 생산자들이 이 기능을 악용한다면 투표에 의해 지위를 상실할 수 있으며 이 경우 동결된 계정은 복원됩니다.

## Changing Account Code

## 계정 코드 변경 (Changing Account Code)

When all else fails and an "unstoppable application" acts in an unpredictable manner, the EOS.IO software allows the block producers to replace the account's code without hard forking the entire blockchain. Similar to the process of freezing an account, this replacement of the code requires a 17/21 vote of elected block producers.

다른 모든 수단이 실패하고 "멈추지 않은 애플리케이션"이 예측할 수 없이 동작한다면 EOS.IO 소프트웨어는 블록 생산자들이 전체 블록체인의 하드 포크 없이 계정의 코드를 교체할 수 있도록 합니다. 계정 동결과 유사하게 코드의 교체 작업은 선출된 블록 생산자들 간의 17/21 투표가 필요합니다.

## Constitution

## 약관 (Constitution)

The EOS.IO software enables blockchains to establish a peer-to-peer terms of service agreement or a binding contract among those users who sign it, referred to as a "constitution". The content of this constitution defines obligations among the users which cannot be entirely enforced by code and facilitates dispute resolution by establishing jurisdiction and choice of law along with other mutually accepted rules. Every transaction broadcast on the network must incorporate the hash of the constitution as part of the signature and thereby explicitly binds the signer to the contract.

EOS.IO 소프트웨어는 블록체인에서 P2P 서비스 약정을 체결하거나, 서명 한 사용자 간의 구속력 있는 계약인 "약관"을 수립하도록 합니다. 약관으로 코드로 제약을 가하기 어려운 사용자 간의 의무를 규정하며, 상호 간의 관할권을 확립하고 법률을 제정함으로 분쟁 해결을 쉽게 합니다. 네트워크로 전파되는 모든 트랜잭션은 약관의 해시(hash)를 서명에 포함해야 하며, 이를 통해 서명자가 명시적으로 계약에 구속되도록 합니다.

The constitution also defines the human-readable intent of the source code protocol. This intent is used to identify the difference between a bug and a feature when errors occur and guides the community on what fixes are proper or improper.   

약관은 사람이 읽을 수 있는 형식으로 소스코드 프로토콜의 의도를 정의합니다. 이를 통해 오류가 발생하였을 때 버그와 기능의 차이를 인식할 수 있도록 하며, 커뮤니티가 수정사항이 적합한지 아닌지 판단하도록 해줍니다.

## Upgrading the Protocol & Constitution

## 프로토콜과 약관의 개정 (Upgrading the Protocol & Constitution)

The EOS.IO software define a process by which the protocol as defined by the canonical source code and its constitution, can be updated using the following process:

EOS.IO 소프트웨어는 소스 코드 원형과 약관으로 규정되는 프로토콜의 절차를 규정하며, 다음의 절차로 개정할 수 있습니다.

1. Block producers propose a change to the constitution and obtains 17/21 approval.

1. 블록 생산자들은 약관의 개정을 제안하고 17/21 승인을 받습니다.

2. Block producers maintain 17/21 approval for 30 consecutive days.

2. 블록 생산자들은 17/21 승인을 30일간 유지합니다.

3. All users are required to sign transactions using the hash of the new constitution.

3. 모든 사용자는 새 약관의 해시를 사용하여 거래에 서명해야 합니다.

4. Block producers adopt changes to the source code to reflect the change in the constitution and propose it to the blockchain using the hash of a git commit.

4. 블록 생산자들은 약관의 변화를 반영하도록 소스 코드의 변경을 채택하며, git 커밋의 해시값을 이용하여 블록체인에 제안합니다.

5. Block producers maintain 17/21 approval for 30 consecutive days.

5. 블록 생산자들은 17/21 승인을 30일간 유지합니다.

6. Changes to the code take effect 7 days later, giving all full nodes 1 week to upgrade after ratification of the source code.

6. 코드 변경은 7일간의 소스코드 적용 유예기간을 주며, 7일이 지난 이후 적용됩니다.

7. All nodes that do not upgrade to the new code shut down automatically.

7. 새 코드로 판올림하지 않은 노드는 강제로 종료됩니다.

By default configuration of the EOS.IO software, the process of updating the blockchain to add new features takes 2 to 3 months, while updates to fix non-critical bugs that do not require changes to the constitution can take 1 to 2 months.

EOS.IO 소프트웨어의 기본 설정에 따르면, 새로운 기능을 추가하는 블록체인의 판올림 작업은 2~3달이 걸리며, 약관의 개정이 필요 없는 치명적이지 않은 버그의 수정은 1~2달 소요됩니다.

### Emergency Changes

### 응급 변경 (Emergency Changes)

The block producers may accelerate the process if a software change is required to fix a harmful bug or security exploit that is actively harming users. Generally speaking it could be against the constitution for accelerated updates to introduce new features or fix harmless bugs.

치명적인 버그나 공격자로 인한 보안 문제가 발생할 경우 블록 생산자는 판올림 과정을 빠르게 해야합니다. 일반적으로 새로운 기능을 도입하거나 치명적이지 않은 버그를 수정하기 위해 판올림을 빠르게 하는 것은 약관에 어긋납니다.

# Scripts & Virtual Machines

# 스크립트와 가상 머신 (Scripts & Virtual Machines)

The EOS.IO software will be first and foremost a platform for coordinating the delivery of authenticated messages to accounts. The details of scripting language and virtual machine are implementation specific details that are mostly independent from the design of the EOS.IO technology.  Any language or virtual machine that is deterministic and properly sandboxed with sufficient performance can be integrated with the EOS.IO software API.

EOS.IO 소프트웨어는 인증된 메시지를 계정으로 전달하는 과정을 조정하는 최초이자 가장 중요한 플랫폼입니다. 스크립트 언어와 가상 머신(virtual machine)의 세부 내용은 EOS.IO의 기술 설계와 독립적인 세부 구현 사항입니다. EOS.IO 소프트웨어 API를 이용하여 어떠한 언어나 성능을 보장하는 샌드박스 처리되며 결정론적으로 동작하는 가상 머신을 통합할 수 있습니다.

## Schema Defined Messages

## 스키마 정의 메시지 (Schema Defined Messages)

All messages sent between accounts are defined by a schema which is part of the blockchain consensus state. This schema allows seamless conversion between binary and JSON representation of the messages.  

계정 간의 모든 메시지는 블록체인 합의 상태의 일부인 스키마(schema)에 따라 정의됩니다. 이 스키마는 바이너리와 JSON 형식의 메시지의 경계 없는(seamless) 대화를 허용합니다.

## Schema Defined Database

## 스키마 정의 데이터베이스 (Schema Defined Database)

Database state is also defined using a similar schema. This ensures that all data stored by all applications is in a format that can be interpreted as human readable JSON but stored and manipulated with the efficiency of binary.

데이터베이스 상태는 유사한 스키마를 이용하여 정의됩니다. 모든 애플리케이션에서 저장되는 모든 데이터는 사람이 읽을 수 있는 JSON으로 처리될 뿐만 아니라 효율적인 바이너리 형태로 저장 관리됨을 보장합니다.

## Separating Authentication from Application

## 애플리케이션과 인증 분리 (Separating Authentication from Application)

To maximize parallelization opportunities and minimize the computational debt associated with regenerating application state from the transaction log, EOS.IO separates validation logic into three sections:

병렬 처리 효율의 극대화와 트랜잭션 로그에서 애플리케이션 상태(state)가 재생성될 때 발생하는 과다 연산을 최소화하기 위하여, EOS.IO는 인증 방법(authentication logic)을 세 가지로 분리합니다.

1. Validating that a message is internally consistent;

1. 메시지의 내적 일관성(internal consistency) 검증

2. Validating that all preconditions are valid; and

2. 모든 전제 조건의 유효성 검증

3. Modifying the application state.

3. 애플리케이션 상태의 변경

Validating the internal consistency of a message is read-only and requires no access to blockchain state. This means that it can be performed with maximum parallelism. Validating preconditions, such as required balance, is read-only and therefore can also benefit from parallelism. Only modification of application state requires write access and must be processed sequentially for each application.

메시지의 내적 일관성 검증은 읽기 연산으로만 구성되며, 블록체인 상태에 대한 확인을 요구하지 않습니다. 이는 최대한의 병렬성을 가질 수 있음을 뜻합니다. 요구불 잔액 확인과 같은 전제 조건의 유효성 검증 역시 읽기 연산만으로 구성되며, 병렬 처리의 이점을 가지게 됩니다. 오직 애플리케이션 상태 변경만 쓰기 연산을 해야 하며, 각각의 애플리케이션마다 순차적으로 처리되어야 합니다.

Authentication is the read-only process of verifying that a message can be applied. Application is actually doing the work. In real time both calculations are required to be performed, however once a transaction is included in the blockchain it is no longer necessary to perform the authentication operations.

인증(authentication)은 메시지의 적용 가능 여부를 검증하는 읽기 연산 작업입니다. 애플리케이션은 실제 작업을 수행합니다. 두 가지 계산은 실시간으로 수행되어야 하지만 트랜잭션이 블록체인에 포함되었다면 더 이상 인증 작업을 수행할 필요가 없습니다.

## Virtual Machine Independent Architecture

## 가상 머신 독립 아키텍처 (Virtual Machine Independent Architecture)

It is the intention of the EOS.IO software that multiple virtual machines can be supported and new virtual machines added over time as necessary. For this reason, this paper will not discuss the details of any particular language or virtual machine. That said, there are two virtual machines that are currently being evaluated for use within EOS.IO.

EOS.IO 소프트웨어의 설계 목적은 다수의 가상 머신을 운용하며 필요에 따라 새로운 가상 머신을 추가하는 것 입니다. 그러므로 본 백서에서는 특정 언어나 가상머신에 세부적인 내용에 관하여 기술하지 않습니다. 현재 EOS.IO를 이용하여 평가 중인 가상 머신은 2가지 입니다.

### Web Assembly (WASM)

### 웹어셈블리 (WASM; Web Assembly)

Web Assembly is an emerging web standard for building high performance web applications. With a few small modifications Web Assembly can be made deterministic and sandboxed. The benefit of Web Assembly is the widespread support from industry and that it enables contracts to be developed in familiar languages such as C or C++.

웹어셈블리는 고성능의 웹 애플리케이션을 제작하기 위하여 새로이 등장한 웹 표준입니다. 웹어셈블리을 일부 수정하여 결정론적이며 샌드박스인 형태로 만들 수 있습니다. 웹어셈블리의 장점은 산업 현장에서 폭넓게 수용되고 있으며, 친숙한 언어인 C나 C++로 컨트렉트를 개발할 수 있습니다.

Ethereum developers have already begun modifying Web Assembly to provide suitable sandboxing and determinism in with their [Ethereum flavored Web Assembly (WASM)](https://github.com/ewasm/design). This approach can be easily adapted and integrated with EOS.IO software.  

이더리움 개발자들은 [Ethereum flavored Web Assembly (EWASM)](https://github.com/ewasm/design)으로 웹어셈블리를 수정하여 샌드박싱하고 결정론적으로 변환하는 작업에 착수하였습니다. EWASM은 EOS.IO 소프트웨어에 쉽게 적용하고 통합할 수 있습니다.

### Ethereum Virtual Machine (EVM)

### 이더리움 가상 머신 (EVM; Ethereum Virtual Machine)

This virtual machine has been used for most existing smart contracts and could be adapted to work within an EOS.IO blockchain.  It is conceivable that EVM contracts could be run within their own sandbox inside an EOS.IO blockchain and that with some adaptation EVM contracts could communicate with other EOS.IO blockchain applications.

이 가상머신은 현존하는 대부분의 스마트 컨트렉트를 구동할 수 있으며, 이를 이용하여 그들의 작업물을 EOS.IO 블록체인에 적용할 수 있습니다. EVM의 컨트렉트는 샌드박스 안에서 구동되며, 약간의 조정을 거치면 EOS.IO의 블록체인 애플리케이션과 통신할 수 있습니다.

# Inter Blockchain Communication

# 블록체인 간 통신 (Inter Blockchain Communication)

EOS.IO software is designed to facilitate inter-blockchain communication. This is achieved by making it easy to generate proof of message existence and proof of message sequence. These proofs combined with an application architecture designed around message passing enables the details of inter-blockchain communication and proof validation to be hidden from application developers.

EOS.IO 소프트웨어는 블록체인 간 통신이 쉽도록 설계되었습니다. 이는 메시지 존재 증명과 메시지 순서 증명의 생성을 손쉽게 함으로 이루어집니다. 이러한 증명들은 메시지 전달을 감싸는 애플리케이션 아키텍처와 결합하여 블록체인간 통신과 증명 검증 과정이 애플리케이션 개발자로부터 은닉되도록 합니다.

## Merkle Proofs for Light Client Validation (LCV)

## 경량화된 클라이언트 검증(LCV)을 위한 머클 증명 (Merkle Proofs for Light Client Validation)

Integrating with other blockchains is much easier if clients do not need to process all transactions.  After all, an exchange only cares about transfers in and out of the exchange and nothing more.  It would also be ideal if the exchange chain could utilize lightweight merkle proofs of deposit rather than having to trust its own block producers entirely. At the very least a chain's block producers would like to maintain the smallest possible overhead when synchronizing with another blockchain.

클라이언트가 모든 트랜잭션을 처리할 필요가 없을 때 블록체인들을 결합하는 것이 쉬워집니다. 사실, 모든 거래소는 전체 블록체인 중 거래소의 입/출금에 대해서만 관리하면 됩니다. 교환 체인(exchange chain)의 입금 이력에 대한 경량화된 머클 증명을 이용하는 것은 블록 생산자 전체에 대한 신뢰성을 유지하는 것 보다 이상적입니다. 적어도 해당 체인의 블록 생산자들은 다른 블록체인과 동기화를 위해 최소한의 연산 부담을 받길 원합니다.

The goal of LCV is to enable the generation of relatively light-weight proof of existence that can be validated by anyone tracking a relatively light-weight data set. In this case the objective is  to prove that a particular transaction was included in a particular block and that the block is included in the verified history of a particular blockchain.  

LCV(경량화된 클라이언트 검증)의 목표는 상대적 경량 데이터 집합을 보고 있는 누구든지 검증할 수 있는 상대적 경량 증명을 만들 수 있게 하는 것입니다. 특정 트랜잭션이 어떤 블록에 포함되어 있고, 그 블록이 블록체인의 인증 이력(verified history)에 포함되어 있는지를 증명하는 것을 목표로 합니다.

Bitcoin supports validation of transactions assuming all nodes have access to the full history of block headers which amounts to 4MB of block headers per year. At 10 transactions per second, a valid proof requires about 512 bytes. This works well for a blockchain with a 10 minute block interval, but is no longer "light" for blockchains with a 3 second block interval.  
 
비트코인은 모든 노드가 블록 헤더에 기록된 4MB 가량의 모든 기록을 접근하는 것을 가정하고 트랜잭션의 검증을 수행합니다. 초당 10개의 트랜잭션이 발생할 경우, 유효한 증명(valid proof)은 512 바이트를 필요로 합니다. 10분의 블록 간격을 가지는 블록체인에서 이 방법은 사용할 수 있지만, 3초의 블록 간격을 가지는 블록체인에는 "경량"이 아닙니다.

The EOS.IO software enables lightweight proofs for anyone who has any irreversible block header after the point in which the transaction was included. Using the hash-linked structure shown below it is possible to prove the existence of any transaction with a proof less than 1024 bytes in size.  If it is  assumed that validating nodes are keeping up with all block headers in the past day (2 MB of data), then proving these transactions will only require proofs 200 bytes long.

EOS.IO 소프트웨어는 트랜잭션이 바뀌지 않는(irreversible) 블록 헤더에 포함된 이후, 해당 블록 헤더를 가진 누구나 경량 증명을 가능하게 합니다. 아래에 보이는 해쉬 연결 구조(hash-linked structure)를 이용하여 1,024바이트 이하의 증명(proof)을 가지는 모든 트랜잭션에 대한 존재 증명(existance prove)이 가능합니다. 유효성 검사 노드가 지난 하루 간(2MB의 데이터)의 모든 블록 헤더를 유지한다 가정하면, 트랜잭션의 검증을 위해서 200바이트의 증명이면 충분합니다.

<img align="right" src="http://eos.io/wpimg/Diagram1.jpg" width="362.84px" height="500px" />

There is little incremental overhead associated with producing blocks with the proper hash-linking to enable these proofs which means there is no reason not to generate blocks this way.

경량 검증 방식을 도입하기 위해 적절한 해쉬-연결(hash-linking)이 포함된 블록을 생성하는 것의 추가 비용은 매우 적으므로, 이 방식으로 블록을 생성하지 않을 이유는 없습니다.

When it comes time to validate proofs on other chains there are a wide variety of time/ space/ bandwidth optimizations that can be made. Tracking all block headers (420 MB/year) will keep proof sizes small.  Tracking only recent headers can offer a trade off between minimal long-term storage and proof size. Alternatively, a blockchain can use a lazy evaluation approach where it remembers intermediate hashes of past proofs. New proofs only have to include links to the known sparse tree. The exact approach used will necessarily depend upon the percentage of foreign blocks that include transactions referenced by merkle proof.  

다른 체인에 속한 증명들을 검증할 경우 다양한 연산 시간/공간/대역폭 최적화가 가능합니다. 모든 블록 헤더(연간 420MB)의 추적(tracking)은 증명의 크기를 작게 유지하게 할 것입니다. 최근 발생한 헤더만 추적하는 것은 장기적 관점의 저장소 관리와 증명 크기 간의 절충이 발생합니다. 대안으로, 블록체인은 과거 증명의 중간 해쉬를 기억하는 지연 평가(lazy evaluation) 방식을 사용할 수 있습니다. 새로운 증명은 이미 알려진 희소 트리(sparse tree)에 대한 링크를 포함하면 됩니다. 어떤 방식을 사용할지는 머클 증명을 참조하는 트랜잭션을 포함하는 외부 블록의 비율에 따라 결정됩니다.

After a certain density of interconnectedness it becomes more efficient to simply have one chain contain the entire block history of another chain and eliminate the need for proofs all together. For performance reasons, it is ideal to minimize the frequency of inter-chain proofs.

상호 연결이 일정 비율 이상 이루어진다면, 단순하게 다른 체인의 모든 블록 기록을 해당 체인이 가지게 하여 검증을 위해 두 체인을 볼 필요가 없게 하는 것이 효율적입니다. 성능 관점에서 체인 간 증명의 비율을 낮추는 것이 좋습니다.

## Latency of Interchain Communication

## 체인 간 통신의 지연 시간 (Latency of Interchain Communication)

When communicating with another outside blockchain, block producers must wait until there is 100% certainty that a transaction has been irreversibly confirmed by the other blockchain before accepting it as a valid input. Using EOS.IO software and DPOS with 3 second blocks and 21 producers, this takes approximately 45 seconds.  If a chain's block producers do not wait for irreversibility it would be like an exchange accepting a deposit that was later reversed and could impact the validity of the a chain's consensus.

외부의 다른 블록체인과 통신할 때, 블록 생산자는 적합한 입력값으로 받아들이기 이전에 다른 블록체인에서 바뀌지 않는 확인 상태에 돌입하였음을 100% 확신할 때까지 기다려야 합니다. EOS.IO 소프트웨어와 21명의 생산자와 3초 블록 생성 시간을 가지는 DPOS(지분 위임 증명; Delegated Proof-Of-Stake)를 이용할 때 약 45초가 걸립니다. 만약에 특정 체인의 블록 생산자가 바뀌지 않는 상태가 되는 것을 기다리지 않는다면 거래소에 취소한 입금이 승인되는 것과 같은 일이 일어나며 체인의 합의 검증에 문제가 발생할 것입니다.

## Proof of Completeness

## 완전성 증명 (Proof of Completeness)

When using merkle proofs from outside blockchains, there is a significant difference between knowing that all transactions processed are valid and knowing that no transactions have been skipped or omitted.  While it is impossible to prove that all of the most recent transactions are known, it is possible to prove that there have been no gaps in the transaction history.  The EOS.IO software facilitates this by assigning a sequence number to every message delivered to every account. A user can use these sequence numbers to prove that all messages intended for a particular account have been processed and that they were processed in order.

블록체인 외부의 머클 증명을 사용할 때, 모든 트랜잭션이 수행 된 것을 확인하는 것과 무시하거나 빠진 트랜잭션이 없음을 확인하는 것은 상당한 차이가 있습니다. 최근의 모든 트랜잭션을 모두 알고 있음을 증명하기는 불가능하지만, 트랜잭션 이력의 빈틈(gap)이 없음을 증명하는 것은 가능합니다. EOS.IO는 모든 계정간 메시지 전달에 순서 번호를 부여하여 이를 가능하게 합니다. 사용자는 특정 계정에 대한 모든 메시지가 정상적으로 처리되었으며, 순서에 맞추어 처리되었음을 증명할 때 이러한 순서 번호를 사용할 수 있습니다.

# Conclusion

# 결론 (Conclusion)

The EOS.IO software is designed from experience with proven concepts and best practices, and represents fundamental advancements in blockchain technology. The software is part of a holistic blueprint for a globally scalable blockchain society in which decentralised applications can be easily deployed and governed.

EOS.IO 소프트웨어는 검증된 개념과 실질적인 경험을 통해 설계되었으며, 블록체인 기술의 근본적인 발전을 대표하는 제품입니다. 이 소프트웨어는 전 세계적 규모의 블록체인의 탈중앙화 애플리케이션을 쉽게 구현하고 운영(governing)하는 전체적인 청사진의 일부입니다.
