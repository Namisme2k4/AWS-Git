---
title: "Blog 1"
date: "2025-09-12"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS AI Agents sẵn sàng sản xuất ở quy mô lớn

## Tổng quan

AWS CEO Matt Garman đã chia sẻ tầm nhìn về một sự thay đổi công nghệ có tính chuyển đổi tương đương với sự ra đời của internet. Bài viết này giới thiệu cách AWS đang xây dựng nền tảng để triển khai AI agents ở quy mô doanh nghiệp với độ tin cậy và bảo mật cao.

![AWS AI Stack Architecture](/images/Blog1.png)
_Figure 1: Ngăn xếp AI của AWS - Nền tảng toàn diện để xây dựng và triển khai các hệ thống AI đại lý sẵn sàng sản xuất ở quy mô lớn._

---

## Bối cảnh và Thách thức

### Thực trạng hiện tại

- Các hệ thống AI agent thông minh đã bắt đầu giải quyết các vấn đề phức tạp, tự động hóa quy trình công việc và tạo ra những khả năng mới trong nhiều ngành
- Thành công ban đầu từ AstraZeneca, Yahoo Finance, Syngenta
- Cần một phương pháp thực tế để giải quyết độ phức tạp vốn có của các hệ thống agentic

### Thách thức chính

- Chuyển từ thử nghiệm sang hệ thống agent sẵn sàng sản xuất
- Địa chỉ độ phức tạp vốn có của agentic systems
- Cân bằng giữa tính linh hoạt và độ tin cậy doanh nghiệp

---

## 4 Nguyên tắc Cốt lõi của AWS

### Nguyên tắc 1: Chấp nhận sự nhanh nhẹn như một lợi thế cạnh tranh

**Tư duy chính:**

- Các tổ chức thành công sẽ không phải là những tổ chức dự đoán hoàn hảo tương lai, mà là những tổ chức thích ứng nhanh chóng khi nó diễn ra
- Xây dựng kiến trúc agentic linh hoạt và mở thay vì các framework cứng nhắc

**Giải pháp: Amazon Bedrock AgentCore**

- Một bộ dịch vụ hoàn chỉnh để triển khai và vận hành các agent có khả năng cao một cách an toàn ở quy mô doanh nghiệp
- Runtime serverless bảo mật với cách ly phiên hoàn toàn
- Tương thích với các framework mã nguồn mở như CrewAI, LangGraph, LlamaIndex
- Khách hàng đã thử nghiệm: Itaú Unibanco, Innovaccer, Boomi, Box, Epsilon

### Nguyên tắc 2: Phát triển các nguyên tắc cơ bản cho kỷ nguyên agentic

**Các yếu tố cơ bản đã phát triển:**

**A. Bảo mật và Tin cậy**

- AgentCore Runtime giúp giải quyết với các môi trường tính toán chuyên dụng cho mỗi phiên và cách ly bộ nhớ giúp ngăn ngừa rò rỉ dữ liệu giữa các agent
- Transparency, guardrails, verification

**B. Độ tin cậy và Khả năng mở rộng**

- Checkpointing và recovery capabilities
- Tự động scaling từ 0 đến hàng nghìn concurrent sessions
- Loại bỏ capacity planning và infrastructure maintenance

**C. Danh tính (Identity)**

- AgentCore Identity với quyền tạm thời, chi tiết
- Tích hợp với Amazon Cognito, Microsoft Entra ID, Okta
- OAuth providers: GitHub, Google, Salesforce, Slack

**D. Observability**

- Khả năng quan sát trở nên cần thiết không chỉ để khắc phục sự cố, mà còn cho việc tuân thủ và cải tiến liên tục
- Real-time visibility qua built-in dashboards
- Standardized telemetry

**E. Dữ liệu**

- AgentCore Gateway chuyển đổi data sources thành agent-compatible tools
- Tích hợp với Amazon Bedrock Knowledge Bases

**F. Tích hợp liền mạch**

- AgentCore Gateway giúp điều này có thể xảy ra bằng cách chuyển đổi API và dịch vụ thành các công cụ tương thích với agent với mã tối thiểu
- AWS API MCP Server cho giao diện callable đến AWS services

**G. Công cụ và Khả năng**

- AgentCore Memory: quản lý short-term và long-term memory
- AgentCore Browser: web interactions
- AgentCore Code Interpreter: thực thi code an toàn

### Nguyên tắc 3: Cung cấp kết quả vượt trội với sự lựa chọn model và dữ liệu

**Tính đa dạng của Model:**

- Không có model đơn lẻ nào xuất sắc trên tất cả các khía cạnh, đó là lý do tại sao chúng tôi đi tiên phong trong việc lựa chọn model với Amazon Bedrock năm 2023

**Khả năng tùy chỉnh mới:**

- **Amazon Nova customization in Amazon SageMaker AI**:
  - Pre-training và post-training
  - Fine-tuning và alignment
  - PEFT và full fine-tuning
  - SFT, DPO, PPO, CPT, Knowledge Distillation
- **Nova Act**: AI model được training để thực hiện actions trong web browser
- **Amazon S3 Vectors**: Giảm chi phí lưu trữ vector 90% trong khi duy trì hiệu suất truy vấn dưới giây

### Nguyên tắc 4: Triển khai các giải pháp chuyển đổi trải nghiệm

**Marketplace và Solutions:**

- AI Agents and Tools trong AWS Marketplace với deployment options linh hoạt
- **Kiro**: AI IDE cho spec-driven development
- **AWS Transform**: tự động hóa modernization tasks
- **Amazon Connect**: unlimited AI trên mọi customer interaction

---

## Các Tính năng và Dịch vụ Chính

### Amazon Bedrock AgentCore

- **Runtime**: Serverless, cách ly session, longest running workload
- **Identity**: Fine-grained permissions, standards-based authentication
- **Memory**: Short-term và long-term memory management
- **Gateway**: Chuyển đổi APIs thành agent-compatible tools
- **Observability**: Real-time monitoring và compliance

### Amazon Nova Models

- **Nova Customization**: Comprehensive model customization capabilities
- **Nova Act**: Browser automation agents (research preview)
- **Nova Act SDK**: Cloud-based browser execution với AgentCore Browser

### Amazon S3 Vectors

- Native vector support trong cloud object store
- 90% cost reduction cho vector storage
- Sub-second query performance
- Tích hợp với Bedrock Knowledge Bases và OpenSearch

---

## Kết quả Thực tế

### Khách hàng Success Stories

- **AstraZeneca**: Accelerated healthcare insight discovery
- **Yahoo Finance**: Transformed financial research for millions
- **Syngenta**: AI-driven precision farming revolution
- **NFL, BMW**: Millions in productivity gains

### Investment và Support

- Đầu tư thêm 100 triệu đô la, tăng gấp đôi đầu tư vào AWS Generative AI Innovation Center
- Hỗ trợ hàng nghìn khách hàng đạt productivity gains hàng triệu đô la

---

## Khuyến nghị Hành động

### Lời khuyên chính

- "Lời khuyên quan trọng nhất tôi có thể đưa ra rất đơn giản: bắt đầu ngay bây giờ"
- Chọn một vấn đề kinh doanh cụ thể quan trọng và bắt đầu xây dựng
- Bắt đầu learning cycle, thu thập feedback thực tế

### Chiến lược triển khai

1. **Không cố gắng giải quyết mọi thứ cùng lúc**
2. **Tập trung vào vấn đề cụ thể**
3. **Lặp lại dựa trên feedback thực tế**
4. **Tận dụng các pre-built solutions từ AWS Marketplace**

---

## Kết luận

AWS đang thiết lập chuẩn mực cho agentic AI với cùng nguyên tắc security, reliability, và data privacy như cloud computing. Bất kể use case hay yêu cầu của bạn, AWS cung cấp nền tảng phù hợp để giúp bạn thành công. Cách tiếp cận này kết hợp rapid innovation với foundation mạnh mẽ về bảo mật và operational excellence, giúp organizations chuyển từ experiments sang production-ready agent systems đáng tin cậy.
