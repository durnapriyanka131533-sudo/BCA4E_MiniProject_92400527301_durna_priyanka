# -----------------------------
# FOOD DELIVERY VISUALIZATION & SAVE PLOTS
# -----------------------------

# 1. Create Dataset
data <- data.frame(
  Order_ID = c("OD001","OD002","OD003","OD004","OD005","OD006"),
  Restaurant_Name = c("Dominos","KFC","Pizza Hut","McDonalds","Subway","Burger King"),
  Category = c("Fast Food","Non-Veg","Fast Food","Fast Food","Veg","Fast Food"),
  Order_Value = c(250,320,450,180,220,300),
  Delivery_Time = c(30,45,25,35,40,50),
  Rating = c(4.2,3.8,4.5,4.0,3.5,4.3),
  City = c("Mumbai","Delhi","Bangalore","Chennai","Hyderabad","Pune"),
  Order_Date = as.Date(c("2023-01-01","2023-02-01","2023-03-01",
                         "2023-04-01","2023-05-01","2023-06-01"))
)

# 2. Load libraries
library(ggplot2)
library(dplyr)
library(corrplot) # for correlation heatmap

# 3. Create folder for saving plots
if(!dir.exists("plots")) dir.create("plots")

# -----------------------------
# Gr01 : Bar Chart – Orders by City
# -----------------------------
p1 <- ggplot(data, aes(x=City)) +
  geom_bar(fill="skyblue") +
  ggtitle("Food Delivery Orders by City") +
  xlab("City") + ylab("Number of Orders")
ggsave("plots/Gr01_Bar_Orders_by_City.png", p1, width=6, height=4)

# -----------------------------
# Gr02 : Pie Chart – Distribution of Food Categories
# -----------------------------
png("plots/Gr02_Pie_Food_Categories.png", width=800, height=600)
pie(table(data$Category), main="Distribution of Food Categories", col=rainbow(length(table(data$Category))))
dev.off()

# -----------------------------
# Gr03 : Grouped Bar Chart – Orders by Category by City
# -----------------------------
p3 <- ggplot(data, aes(x=City, fill=Category)) +
  geom_bar(position="dodge") +
  ggtitle("Orders by Category by City") +
  ylab("Number of Orders") + xlab("City")
ggsave("plots/Gr03_GroupedBar_Orders_by_Category_City.png", p3, width=7, height=5)

# -----------------------------
# Gr04 : Boxplot – Order Value Variation by Category
# -----------------------------
p4 <- ggplot(data, aes(x=Category, y=Order_Value, fill=Category)) +
  geom_boxplot() +
  ggtitle("Order Value Variation by Category") +
  xlab("Category") + ylab("Order Value")
ggsave("plots/Gr04_Boxplot_OrderValue_by_Category.png", p4, width=6, height=4)

# -----------------------------
# Gr05 : Line Chart – Yearly Average Order Value
# -----------------------------
data$Year <- format(data$Order_Date, "%Y")
yearly_avg <- data %>% group_by(Year) %>% summarise(Average_Order_Value = mean(Order_Value))
p5 <- ggplot(yearly_avg, aes(x=Year, y=Average_Order_Value, group=1)) +
  geom_line(color="blue") + geom_point(color="red") +
  ggtitle("Yearly Average Order Value") +
  xlab("Year") + ylab("Average Order Value")
ggsave("plots/Gr05_Line_Yearly_Avg_OrderValue.png", p5, width=6, height=4)

# -----------------------------
# Gr06 : Dot Chart – Average Delivery Time by City
# -----------------------------
avg_delivery <- data %>% group_by(City) %>% summarise(Average_Delivery_Time = mean(Delivery_Time))
p6 <- ggplot(avg_delivery, aes(x=Average_Delivery_Time, y=City)) +
  geom_point(color="darkgreen", size=4) +
  ggtitle("Average Delivery Time by City") +
  xlab("Delivery Time (minutes)") + ylab("City")
ggsave("plots/Gr06_Dot_Avg_DeliveryTime_by_City.png", p6, width=6, height=4)

# -----------------------------
# Gr07 : Bar Chart – Average Order Value by City
# -----------------------------
avg_order_city <- data %>% group_by(City) %>% summarise(Average_Order_Value = mean(Order_Value))
p7 <- ggplot(avg_order_city, aes(x=City, y=Average_Order_Value, fill=City)) +
  geom_bar(stat="identity") +
  ggtitle("Average Order Value by City") +
  xlab("City") + ylab("Average Order Value")
ggsave("plots/Gr07_Bar_Avg_OrderValue_by_City.png", p7, width=6, height=4)

# -----------------------------
# Gr08 : Histogram – Order Value Distribution
# -----------------------------
p8 <- ggplot(data, aes(x=Order_Value)) +
  geom_histogram(binwidth=50, fill="orange", color="black") +
  ggtitle("Order Value Distribution") + xlab("Order Value") + ylab("Frequency")
ggsave("plots/Gr08_Histogram_OrderValue.png", p8, width=6, height=4)

# -----------------------------
# Gr09 : Scatter Plot – Average Delivery Time vs Avg Order Value
# -----------------------------
city_avg <- data %>% group_by(City) %>% summarise(Average_Order_Value = mean(Order_Value),
                                                  Average_Delivery_Time = mean(Delivery_Time))
p9 <- ggplot(city_avg, aes(x=Average_Order_Value, y=Average_Delivery_Time, label=City)) +
  geom_point(color="purple", size=4) +
  geom_text(vjust=-0.5) +
  ggtitle("Average Delivery Time vs Avg Order Value by City") +
  xlab("Average Order Value") + ylab("Average Delivery Time (min)")
ggsave("plots/Gr09_Scatter_AvgDelivery_vs_OrderValue.png", p9, width=6, height=4)

# -----------------------------
# Gr10 – Histograms: Order Value, Delivery Time, Ratings
# -----------------------------
png("plots/Gr10_Histograms_OrderValue_Delivery_Rating.png", width=1200, height=400)
par(mfrow=c(1,3))
hist(data$Order_Value, main="Order Value", xlab="Order Value", col="orange")
hist(data$Delivery_Time, main="Delivery Time", xlab="Minutes", col="lightblue")
hist(data$Rating, main="Ratings", xlab="Rating", col="lightgreen")
par(mfrow=c(1,1))
dev.off()

# -----------------------------
# Gr11: Multiline Chart – City-wise Yearly Average Order Value
# -----------------------------
city_year_avg <- data %>% group_by(City, Year) %>% summarise(Avg_Order_Value=mean(Order_Value))
p11 <- ggplot(city_year_avg, aes(x=Year, y=Avg_Order_Value, group=City, color=City)) +
  geom_line() + geom_point() +
  ggtitle("City-wise Yearly Average Order Value") +
  xlab("Year") + ylab("Average Order Value")
ggsave("plots/Gr11_Multiline_City_Yearly_Avg_OrderValue.png", p11, width=6, height=4)

# -----------------------------
# Gr12: Stacked Bar Chart – Monthly Order Value Trend by City
# -----------------------------
data$Month <- format(data$Order_Date, "%b")
monthly_city <- data %>% group_by(Month, City) %>% summarise(Monthly_Order_Value=sum(Order_Value))
p12 <- ggplot(monthly_city, aes(x=Month, y=Monthly_Order_Value, fill=City)) +
  geom_bar(stat="identity") +
  ggtitle("Monthly Order Value Trend by City") +
  ylab("Order Value") + xlab("Month")
ggsave("plots/Gr12_StackedBar_Monthly_OrderValue_by_City.png", p12, width=7, height=5)

# -----------------------------
# Gr13: Correlation Heatmap – Order, Delivery, Rating
# -----------------------------
cor_mat <- cor(data[,c("Order_Value","Delivery_Time","Rating")])
png("plots/Gr13_Correlation_Heatmap.png", width=600, height=600)
corrplot(cor_mat, method="color", addCoef.col="black", tl.cex=1.2, number.cex=1.2, title="Correlation Heatmap", mar=c(0,0,1,0))
dev.off()

# -----------------------------
# Gr14: Line Chart – Order Value Bucket Trend by City
# -----------------------------
data$Order_Bucket <- cut(data$Order_Value, breaks=c(0,200,400,600,Inf), labels=c("0-200","201-400","401-600","601+"))
bucket_trend <- data %>% group_by(Year, City, Order_Bucket) %>% summarise(Count=n())
p14 <- ggplot(bucket_trend, aes(x=Year, y=Count, color=Order_Bucket, group=Order_Bucket)) +
  geom_line() + facet_wrap(~City) +
  ggtitle("Order Value Bucket Trend by City") +
  ylab("Number of Orders") + xlab("Year")
ggsave("plots/Gr14_Line_OrderValue_Bucket_Trend.png", p14, width=8, height=5)

# -----------------------------
# Gr15: Pie Charts – Order Value, Delivery Time, Ratings Levels
# -----------------------------
data$Order_Value_Level <- cut(data$Order_Value, breaks=c(0,200,400,600,Inf), labels=c("Low","Medium","High","Very High"))
data$Delivery_Time_Level <- cut(data$Delivery_Time, breaks=c(0,20,40,60,Inf), labels=c("Fast","Moderate","Slow","Very Slow"))
data$Rating_Level <- cut(data$Rating, breaks=c(0,2,3.5,4.5,5), labels=c("Poor","Average","Good","Excellent"))

png("plots/Gr15_Pie_OrderValue_Delivery_Rating.png", width=1200, height=400)
par(mfrow=c(1,3))
pie(table(data$Order_Value_Level), main="Order Value Levels", col=rainbow(4))
pie(table(data$Delivery_Time_Level), main="Delivery Time Levels", col=rainbow(4))
pie(table(data$Rating_Level), main="Ratings Levels", col=rainbow(4))
par(mfrow=c(1,1))
dev.off()