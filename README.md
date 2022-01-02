# Design-a-Client-Investment-Database-in-SQL-developer
Develop investment portfolio, provide insights to record, track  and analyze client investments pertaining to mutual funds and stock shares. 

## The below ER diagram displays the investment portfolio case study.
![image](https://user-images.githubusercontent.com/75762778/147888409-49191738-6dec-425f-ba30-8a776e4dedb1.png)

**Database Creation**
```
CREATE TABLE Client_T 
( 
TaxPayerID VARCHAR2(11) NOT NULL,
StreetAddress VARCHAR2(100), 
State VARCHAR2(50), 
City VARCHAR2(100), 
ZipCode VARCHAR2(10), 
ClientType VARCHAR2(2),
CONSTRAINT Check_Client CHECK (ClientType IN ('B','I')),
CONSTRAINT PK_TaxPayerID PRIMARY KEY (TaxPayerID) 
);
CREATE TABLE Individual_T (
TaxPayerID VARCHAR2(11) NOT NULL,
FirstName VARCHAR2(100),
LastName VARCHAR2(100),
DateOfBirth DATE,
CONSTRAINT Individual_PK PRIMARY KEY (TaxPayerId),
CONSTRAINT Individual_FK FOREIGN KEY (TaxPayerId) REFERENCES Client_T(TaxPayerId)
);
CREATE TABLE Business_T (
TaxPayerID VARCHAR2(11) NOT NULL,
BusinessName VARCHAR2(100),
CONSTRAINT Business_PK PRIMARY KEY (TaxPayerId),
CONSTRAINT Business_FK FOREIGN KEY (TaxPayerId) REFERENCES Client_T(TaxPayerId)
);
CREATE TABLE Stock_T 
(StockTicker VARCHAR2(10) NOT NULL, 
StockName VARCHAR2(50) NOT NULL, 
PrincipalBusiness NUMBER(10), 
CurrentPrice NUMBER(10,2), 
LowPrice NUMBER(10,2),
HighPrice NUMBER(10,2), 
PriorYearAppreciation NUMBER(10,2),
FiveYearAppreciation NUMBER(10,2),
Rating VARCHAR2 (10),
CONSTRAINT PK_Stock PRIMARY KEY(StockTicker) 
);
CREATE TABLE StockPortfolio_T 
(TaxPayerID VARCHAR2(11), 
StockTicker VARCHAR2(10) NOT NULL, 
StockTotalShares NUMBER(10) NOT NULL, 
CONSTRAINT PK_StockPortfolio PRIMARY KEY(TaxPayerID,StockTicker), 
CONSTRAINT FK_StockPortfolioClient FOREIGN KEY(TaxPayerID) REFERENCES Client_T(TaxPayerID),
CONSTRAINT FK_Stock FOREIGN KEY(StockTicker) REFERENCES Stock_T(StockTicker));
CREATE TABLE FundFamily_T
(
FundFamilyID INTEGER NOT NULL,
CompanyName VARCHAR2(50) NOT NULL, 
StreetAddress VARCHAR2(100),
City VARCHAR2(100),
State VARCHAR2(50),
Zip VARCHAR2(10),
CONSTRAINT PK_FundFamily PRIMARY KEY (FundFamilyID)
);
CREATE TABLE MutualFund_T 
(
MFTicker VARCHAR2 (10) Not Null,
FundName varchar2 (20), 
PercentYield Number (3,2), 
LowPrice Number (10,2), 
HighPrice Number (10,2), 
CurrentPrice Number (10,2) ,
PrincipalObjective varchar2 (20),
FUNDFAMILYID VARCHAR2(10)Not Null,
CONSTRAINT MF_FundName Unique (FundName),
CONSTRAINT PK_MutualFund PRIMARY KEY(MFTicker), 
CONSTRAINT FK_MutualFundFundFamily FOREIGN KEY (FUNDFAMILYID) 
REFERENCES FundFamily_T(FUNDFAMILYID) 
);
CREATE TABLE MFPortfolio_T 
(TaxPayerID VARCHAR2(11)Not Null,
MFTicker VARCHAR2(10) NOT NULL, 
MFTotalShares NUMBER(10,2),
CONSTRAINT PK_MFPortfolio PRIMARY KEY(TaxPayerID,MFTicker), 
CONSTRAINT FK1_MFPortfolioClient_T FOREIGN KEY(TaxPayerID) REFERENCES Client_T (TaxPayerID), 
CONSTRAINT FK2_MFPortfolioMutualFund FOREIGN KEY(MFTicker) REFERENCES MutualFund_T(MFTicker) 
);
```


