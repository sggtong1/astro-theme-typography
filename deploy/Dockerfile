# 构建阶段 - 通过 AS builder 命名这个阶段
FROM node:18-alpine AS builder  # 第一个 FROM 开始构建阶段
WORKDIR /app                    # 设置工作目录
COPY package.json pnpm-lock.yaml ./  # 复制依赖文件
RUN npm install -g pnpm        # 安装 pnpm
RUN pnpm install              # 安装项目依赖
COPY . .                      # 复制所有源代码
RUN pnpm build               # 构建项目，生成 dist 目录

# 运行阶段 - 通过新的 FROM 指令开始
FROM nginx:alpine            # 第二个 FROM 开始运行阶段
# 从构建阶段复制构建结果到运行阶段
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
