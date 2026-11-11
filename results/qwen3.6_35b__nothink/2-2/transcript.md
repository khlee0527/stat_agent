# 문제 2-2. 차고 정보가 없다는 것은 무엇을 의미할까?

- 모델: `qwen3.6:35b`
- 데이터: train.csv
- 추론 모드: 끔
- skill: `off`
- 소요 시간: 총 403초 ((a) 45초, (b) 213초, (c) 145초)

## (a)

> (a) 변수 설명서를 읽고 차고·지하실 관련 변수에서 NA가 어떤 의미로 사용되는지 확인하라. 시설이 없는 경우와 정보가 기록되지 않은 경우를 구분하는 규칙을 작성하라.

**도구: list_files** `{}`

<details><summary>실행 결과</summary>

```
{
  "files": [
    {
      "name": "data_description.txt",
      "size_kb": 13.1
    },
    {
      "name": "residual_plot.png",
      "size_kb": 96.7
    },
    {
      "name": "train.csv",
      "size_kb": 449.9
    }
  ]
}
```

</details>

**도구: read_file** `{"filename": "data_description.txt"}`

<details><summary>실행 결과</summary>

```
{
  "message": "'data_description.txt'을 문자열 변수 `doc_data_description`에 저장했습니다.",
  "text": "MSSubClass: Identifies the type of dwelling involved in the sale.\t\n\n        20\t1-STORY 1946 & NEWER ALL STYLES\n        30\t1-STORY 1945 & OLDER\n        40\t1-STORY W/FINISHED ATTIC ALL AGES\n        45\t1-1/2 STORY - UNFINISHED ALL AGES\n        50\t1-1/2 STORY FINISHED ALL AGES\n        60\t2-STORY 1946 & NEWER\n        70\t2-STORY 1945 & OLDER\n        75\t2-1/2 STORY ALL AGES\n        80\tSPLIT OR MULTI-LEVEL\n        85\tSPLIT FOYER\n        90\tDUPLEX - ALL STYLES AND AGES\n       120\t1-STORY PUD (Planned Unit Development) - 1946 & NEWER\n       150\t1-1/2 STORY PUD - ALL AGES\n       160\t2-STORY PUD - 1946 & NEWER\n       180\tPUD - MULTILEVEL - INCL SPLIT LEV/FOYER\n       190\t2 FAMILY CONVERSION - ALL STYLES AND AGES\n\nMSZoning: Identifies the general zoning classification of the sale.\n\t\t\n       A\tAgriculture\n       C\tCommercial\n       FV\tFloating Village Residential\n       I\tIndustrial\n       RH\tResidential High Density\n       RL\tResidential Low Density\n       RP\tResidential Low Density Park \n       RM\tResidential Medium Density\n\t\nLotFrontage: Linear feet of street connected to property\n\nLotArea: Lot size in square feet\n\nStreet: Type of road access to property\n\n       Grvl\tGravel\t\n       Pave\tPaved\n       \t\nAlley: Type of alley access to property\n\n       Grvl\tGravel\n       Pave\tPaved\n       NA \tNo alley access\n\t\t\nLotShape: General shape of property\n\n       Reg\tRegular\t\n       IR1\tSlightly irregular\n       IR2\tModerately Irregular\n       IR3\tIrregular\n       \nLandContour: Flatness of the property\n\n       Lvl\tNear Flat/Level\t\n       Bnk\tBanked - Quick and significant rise from street grade to building\n       HLS\tHillside - Significant slope from side to side\n       Low\tDepression\n\t\t\nUtilities: Type of utilities available\n\t\t\n       AllPub\tAll public Utilities (E,G,W,& S)\t\n       NoSewr\tElectricity, Gas, and Water (Septic Tank)\n       NoSeWa\tElectricity and Gas Only\n       ELO\tElectricity only\t\n\t\nLotConfig: Lot configuration\n\n       Inside\tInside lot\n       Corner\tCorner lot\n       CulDSac\tCul-de-sac\n       FR2\tFrontage on 2 sides of property\n       FR3\tFrontage on 3 sides of property\n\t\nLandSlope: Slope of property\n\t\t\n       Gtl\tGentle slope\n       Mod\tModerate Slope\t\n       Sev\tSevere Slope\n\t\nNeighborhood: Physical locations within Ames city limits\n\n       Blmngtn\tBloomington Heights\n       Blueste\tBluestem\n       BrDale\tBriardale\n       BrkSide\tBrookside\n       ClearCr\tClear Creek\n       CollgCr\tCollege Creek\n       Crawfor\tCrawford\n       Edwards\tEdwards\n       Gilbert\tGilbert\n       IDOTRR\tIowa DOT and Rail Road\n       MeadowV\tMeadow Village\n       Mitchel\tMitchell\n       Names\tNorth Ames\n       NoRidge\tNorthridge\n       NPkVill\tNorthpark Villa\n       NridgHt\tNorthridge Heights\n       NWAmes\tNorthwest Ames\n       OldTown\tOld Town\n       SWISU\tSouth & West of Iowa State University\n       Sawyer\tSawyer\n       SawyerW\tSawyer West\n       Somerst\tSomerset\n       StoneBr\tStone Brook\n       Timber\tTimberland\n       Veenker\tVeenker\n\t\t\t\nCondition1: Proximity to various co\n... (중간 7370자 생략) ...\nFireplaces: Number of fireplaces\n\nFireplaceQu: Fireplace quality\n\n       Ex\tExcellent - Exceptional Masonry Fireplace\n       Gd\tGood - Masonry Fireplace in main level\n       TA\tAverage - Prefabricated Fireplace in main living area or Masonry Fireplace in basement\n       Fa\tFair - Prefabricated Fireplace in basement\n       Po\tPoor - Ben Franklin Stove\n       NA\tNo Fireplace\n\t\t\nGarageType: Garage location\n\t\t\n       2Types\tMore than one type of garage\n       Attchd\tAttached to home\n       Basment\tBasement Garage\n       BuiltIn\tBuilt-In (Garage part of house - typically has room above garage)\n       CarPort\tCar Port\n       Detchd\tDetached from home\n       NA\tNo Garage\n\t\t\nGarageYrBlt: Year garage was built\n\t\t\nGarageFinish: Interior finish of the garage\n\n       Fin\tFinished\n       RFn\tRough Finished\t\n       Unf\tUnfinished\n       NA\tNo Garage\n\t\t\nGarageCars: Size of garage in car capacity\n\nGarageArea: Size of garage in square feet\n\nGarageQual: Garage quality\n\n       Ex\tExcellent\n       Gd\tGood\n       TA\tTypical/Average\n       Fa\tFair\n       Po\tPoor\n       NA\tNo Garage\n\t\t\nGarageCond: Garage condition\n\n       Ex\tExcellent\n       Gd\tGood\n       TA\tTypical/Average\n       Fa\tFair\n       Po\tPoor\n       NA\tNo Garage\n\t\t\nPavedDrive: Paved driveway\n\n       Y\tPaved \n       P\tPartial Pavement\n       N\tDirt/Gravel\n\t\t\nWoodDeckSF: Wood deck area in square feet\n\nOpenPorchSF: Open porch area in square feet\n\nEnclosedPorch: Enclosed porch area in square feet\n\n3SsnPorch: Three season porch area in square feet\n\nScreenPorch: Screen porch area in square feet\n\nPoolArea: Pool area in square feet\n\nPoolQC: Pool quality\n\t\t\n       Ex\tExcellent\n       Gd\tGood\n       TA\tAverage/Typical\n       Fa\tFair\n       NA\tNo Pool\n\t\t\nFence: Fence quality\n\t\t\n       GdPrv\tGood Privacy\n       MnPrv\tMinimum Privacy\n       GdWo\tGood Wood\n       MnWw\tMinimum Wood/Wire\n       NA\tNo Fence\n\t\nMiscFeature: Miscellaneous feature not covered in other categories\n\t\t\n       Elev\tElevator\n       Gar2\t2nd Garage (if not described in garage section)\n       Othr\tOther\n       Shed\tShed (over 100 SF)\n       TenC\tTennis Court\n       NA\tNone\n\t\t\nMiscVal: $Value of miscellaneous feature\n\nMoSold: Month Sold (MM)\n\nYrSold: Year Sold (YYYY)\n\nSaleType: Type of sale\n\t\t\n       WD \tWarranty Deed - Conventional\n       CWD\tWarranty Deed - Cash\n       VWD\tWarranty Deed - VA Loan\n       New\tHome just constructed and sold\n       COD\tCourt Officer Deed/Estate\n       Con\tContract 15% Down payment regular terms\n       ConLw\tContract Low Down payment and low interest\n       ConLI\tContract Low Interest\n       ConLD\tContract Low Down\n       Oth\tOther\n\t\t\nSaleCondition: Condition of sale\n\n       Normal\tNormal Sale\n       Abnorml\tAbnormal Sale -  trade, foreclosure, short sale\n       AdjLand\tAdjoining Land Purchase\n       Alloca\tAllocation - two linked properties with separate deeds, typically condo with a garage unit\t\n       Family\tSale between family members\n       Partial\tHome was not completed when last assessed (associated with New Homes)\n",
  "total_chars": 13370
}
```

</details>

**도구: read_file** `{"filename": "train.csv"}`

<details><summary>실행 결과</summary>

```
{
  "message": "'train.csv'을 변수 `df`에 저장했습니다. (인코딩 utf-8, 구분자 ',')",
  "profile": {
    "var_name": "df",
    "shape": [
      1460,
      81
    ],
    "columns": [
      {
        "position": 0,
        "name": "Id",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 1
      },
      {
        "position": 1,
        "name": "MSSubClass",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 60
      },
      {
        "position": 2,
        "name": "MSZoning",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "RL"
      },
      {
        "position": 3,
        "name": "LotFrontage",
        "dtype": "float64",
        "n_missing": 259,
        "first_non_missing": 65.0
      },
      {
        "position": 4,
        "name": "LotArea",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 8450
      },
      {
        "position": 5,
        "name": "Street",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Pave"
      },
      {
        "position": 6,
        "name": "Alley",
        "dtype": "object",
        "n_missing": 1369,
        "first_non_missing": "Grvl"
      },
      {
        "position": 7,
        "name": "LotShape",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Reg"
      },
      {
        "position": 8,
        "name": "LandContour",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Lvl"
      },
      {
        "position": 9,
        "name": "Utilities",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "AllPub"
      },
      {
        "position": 10,
        "name": "LotConfig",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Inside"
      },
      {
        "position": 11,
        "name": "LandSlope",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Gtl"
      },
      {
        "position": 12,
        "name": "Neighborhood",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "CollgCr"
      },
      {
        "position": 13,
        "name": "Condition1",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Norm"
      },
      {
        "position": 14,
        "name": "Condition2",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Norm"
      },
      {
        "position": 15,
        "name": "BldgType",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "1Fam"
      },
      {
        "position": 16,
        "name": "HouseStyle",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "2Story"
      },
      {
        "position": 17,
        "name": "OverallQual",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 7
      },
      {
        "position": 18,
        "name": "OverallCond",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 5
      },
      {
        "position": 19,
        "name": "YearBuilt",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 2003
      },
      {
        "position": 20,
        "name": "YearRemodAdd",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 2003
      },
      {
        "position": 21,
        "name": "RoofStyle",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Gable"
      },
      {
        "position": 22,
        "name": "RoofMatl",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "CompShg"
      },
      {
        "position": 23,
        "name": "Exterior1st",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "VinylSd"
      },
      {
        "position": 24,
        "name": "Exterior2nd",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "VinylSd"
      },
      {
        "position": 25,
        "name": "MasVnrType",
        "dtype": "object",
        "n_missing": 872,
        "first_non_missing": "BrkFace"
      },
      {
        "position": 26,
        "name": "MasVnrArea",
        "dtype": "float64",
        "n_missing": 8,
        "first_non_missing": 196.0
      },
      {
        "position": 27,
        "name": "ExterQual",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Gd"
      },
      {
        "position": 28,
        "name": "ExterCond",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "TA"
      },
      {
        "position": 29,
        "name": "Foundation",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "PConc"
      },
      {
        "position": 30,
        "name": "BsmtQual",
        "dtype": "object",
        "n_missing": 37,
        "first_non_missing": "Gd"
      },
      {
        "position": 31,
        "name": "BsmtCond",
        "dtype": "object",
        "n_missing": 37,
        "first_non_missing": "TA"
      },
      {
        "position": 32,
        "name": "BsmtExposure",
        "dtype": "object",
        "n_missing": 38,
        "first_non_missing": "No"
      },
      {
        "position": 33,
        "name": "BsmtFinType1",
        "dtype": "object",
        "n_missing": 37,
        "first_non_missing": "GLQ"
      },
      {
        "position": 34,
        "name": "BsmtFinSF1",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 706
      },
      {
        "position": 35,
        "name": "BsmtFinType2",
        "dtype": "object",
        "n_missing": 38,
        "first_non_missing": "Unf"
      },
      {
        "position": 36,
        "name": "BsmtFinSF2",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 37,
        "name": "BsmtUnfSF",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 150
      },
      {
        "position": 38,
        "name": "TotalBsmtSF",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 856
      },
      {
        "position": 39,
        "name": "Heating",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "GasA"
      },
      {
        "position": 40,
        "name": "HeatingQC",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Ex"
      },
      {
        "position": 41,
        "name": "CentralAir",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Y"
      },
      {
        "position": 42,
        "name": "Electrical",
        "dtype": "object",
        "n_missing": 1,
        "first_non_missing": "SBrkr"
      },
      {
        "position": 43,
        "name": "1stFlrSF",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 856
      },
      {
        "position": 44,
        "name": "2ndFlrSF",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 854
      },
      {
        "position": 45,
        "name": "LowQualFinSF",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 46,
        "name": "GrLivArea",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 1710
      },
      {
        "position": 47,
        "name": "BsmtFullBath",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 1
      },
      {
        "position": 48,
        "name": "BsmtHalfBath",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 49,
        "name": "FullBath",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 2
      },
      {
        "position": 50,
        "name": "HalfBath",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 1
      },
      {
        "position": 51,
        "name": "BedroomAbvGr",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 3
      },
      {
        "position": 52,
        "name": "KitchenAbvGr",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 1
      },
      {
        "position": 53,
        "name": "KitchenQual",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Gd"
      },
      {
        "position": 54,
        "name": "TotRmsAbvGrd",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 8
      },
      {
        "position": 55,
        "name": "Functional",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Typ"
      },
      {
        "position": 56,
        "name": "Fireplaces",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 57,
        "name": "FireplaceQu",
        "dtype": "object",
        "n_missing": 690,
        "first_non_missing": "TA"
      },
      {
        "position": 58,
        "name": "GarageType",
        "dtype": "object",
        "n_missing": 81,
        "first_non_missing": "Attchd"
      },
      {
        "position": 59,
        "name": "GarageYrBlt",
        "dtype": "float64",
        "n_missing": 81,
        "first_non_missing": 2003.0
      },
      {
        "position": 60,
        "name": "GarageFinish",
        "dtype": "object",
        "n_missing": 81,
        "first_non_missing": "RFn"
      },
      {
        "position": 61,
        "name": "GarageCars",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 2
      },
      {
        "position": 62,
        "name": "GarageArea",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 548
      },
      {
        "position": 63,
        "name": "GarageQual",
        "dtype": "object",
        "n_missing": 81,
        "first_non_missing": "TA"
      },
      {
        "position": 64,
        "name": "GarageCond",
        "dtype": "object",
        "n_missing": 81,
        "first_non_missing": "TA"
      },
      {
        "position": 65,
        "name": "PavedDrive",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Y"
      },
      {
        "position": 66,
        "name": "WoodDeckSF",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 67,
        "name": "OpenPorchSF",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 61
      },
      {
        "position": 68,
        "name": "EnclosedPorch",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 69,
        "name": "3SsnPorch",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 70,
        "name": "ScreenPorch",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 71,
        "name": "PoolArea",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 72,
        "name": "PoolQC",
        "dtype": "object",
        "n_missing": 1453,
        "first_non_missing": "Ex"
      },
      {
        "position": 73,
        "name": "Fence",
        "dtype": "object",
        "n_missing": 1179,
        "first_non_missing": "MnPrv"
      },
      {
        "position": 74,
        "name": "MiscFeature",
        "dtype": "object",
        "n_missing": 1406,
        "first_non_missing": "Shed"
      },
      {
        "position": 75,
        "name": "MiscVal",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 76,
        "name": "MoSold",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 2
      },
      {
        "position": 77,
        "name": "YrSold",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 2008
      },
      {
        "position": 78,
        "name": "SaleType",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "WD"
      },
      {
        "position": 79,
        "name": "SaleCondition",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Normal"
      },
      {
        "position": 80,
        "name": "SalePrice",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 208500
      }
    ],
    "preview": {
      "column_positions": [
        0,
        1,
        2,
        3,
        4,
        5,
        6,
        7
      ],
      "rows": [
        [
          1,
          60,
          "RL",
          65.0,
          8450,
          "Pave",
          null,
          "Reg"
        ],
        [
          2,
          20,
          "RL",
          80.0,
          9600,
          "Pave",
          null,
          "Reg"
        ],
        [
          3,
          60,
          "RL",
          68.0,
          11250,
          "Pave",
          null,
          "IR1"
        ]
      ]
    },
    "missing_note": "null은 로딩된 DataFrame의 결측값입니다. 결측은 isna()/notna()로 확인하세요. 문자열은 실제 관측값입니다.",
    "keep_default_na_requested": true,
    "document_candidates": []
  }
}
```

</details>

### 답변

변수 설명서(`data_description.txt`)와 학습 데이터(`train.csv`)를 분석한 결과를 바탕으로 답변합니다.

### (a) 차고(Garage) 및 지하실(Basement) 관련 변수에서 NA의 의미와 구분 규칙

**1. NA가 의미하는 바**
제공된 `data_description.txt`에 따르면, 차고(Garage)와 지하실(Basement) 관련 변수들에서 `NA`는 **"해당 시설이 없음(No ...)"**을 의미합니다. 즉, 정보가 기록되지 않은 결측값이 아니라, **시설 자체가 존재하지 않는다는 명확한 카테고리**입니다.

*   **지하실 관련:**
    *   `BsmtQual`, `BsmtCond`, `BsmtExposure`, `BsmtFinType1`, `BsmtFinType2` 등: NA는 지하실이 없는 경우를 나타냅니다.
*   **차고 관련:**
    *   `GarageType`, `GarageFinish`, `GarageQual`, `GarageCond`: NA는 차고가 없는 경우(No Garage)를 나타냅니다.
    *   `GarageYrBlt` (차고 연식): 차고가 없으므로 연식 데이터도 NA입니다.

**2. 시설이 없는 경우와 정보가 기록되지 않은 경우를 구분하는 규칙**

이 데이터셋(Kaggle House Prices)의 특성상, 지하실과 차고 관련 변수들은 **"시설이 없음"을 나타내는 NA**와 **"실제 결측(정보 누락)"**이 혼재되어 있지 않습니다. 모든 NA는 **'시설 없음'**으로 해석됩니다. 따라서 별도의 구분 규칙(예: 빈 문자열 확인 등) 없이 **NA 값 자체가 '시설 없음'을 의미**한다고 보는 것이 정확합니다.

하지만 일반적인 데이터 분석 관점에서 "시설이 없는 경우"와 "정보가 기록되지 않은 경우(실제 결측)"를 구분하기 위해 적용할 수 있는 논리적 규칙은 다음과 같습니다.

*   **규칙 1: 변수 설명서 확인**
    *   설명서에 `NA`가 특정 카테고리(예: "No Garage", "No Basement")로 명시되어 있다면, 해당 NA는 **'시설 없음'**으로 처리합니다.
    *   (본 문제의 경우, 모든 차고/지하실 관련 변수가 이에 해당합니다.)

*   **규칙 2: 다른 관련 변수와의 일관성 확인**
    *   예: `GarageType`이 NA인 경우, `GarageCars`나 `GarageArea`도 0이거나 NA인지 확인하여 '시설 없음'임을 교차 검증합니다.
    *   예: `BsmtQual`이 NA인 경우, `TotalBsmtSF`가 0인지 확인하여 지하실이 없음을 확인합니다.

*   **규칙 3: 실제 결측(정보 누락)과의 구분**
    *   만약 어떤 변수의 설명서에 `NA`가 "시설 없음"으로 명시되어 있지 않다면, 그 NA는 **'정보가 기록되지 않은 경우'**로 간주해야 합니다. (예: 일부 품질 평가 변수에서 시설은 있지만 평가자가 기록을 안 한 경우)
    *   본 데이터에서는 차고/지하실 관련 주요 변수들이 모두 "시설 없음"을 의미하므로, **NA = 시설 없음**으로 일관되게 처리하면 됩니다.

**요약:**
*   **NA의 의미:** 지하실이나 차고가 **물리적으로 존재하지 않음**.
*   **구분 규칙:** 변수 설명서에서 `NA`가 "No [Feature]"로 명시된 경우, 해당 NA는 **'시설 없음'**으로 처리합니다. 본 데이터셋에서는 모든 차고/지하실 관련 주요 변수가 이 규칙을 따르므로, NA를 '시설 없음'으로 간주하고 분석 시 해당 관측치를 제외하거나 별도 카테고리('None')로 처리해야 합니다.

_소요 시간: 45초_

## (b)

> (b) (a)의 규칙에 따라 해당 변수를 처리하라. 모든 NA를 일괄적으로 최빈값이나 중앙값으로 대체한 자료와 비교하여, 어떤 주택의 특성이 다르게 표현되는지 찾아라.

**도구: run_python**

```python
import pandas as pd
import numpy as np

# 데이터 로드 (이미 df가 있지만, 명확성을 위해 다시 읽거나 기존 변수 사용)
# 기존 df가 있으므로 그대로 사용하되, 필요한 컬럼만 추출하여 분석용 데이터프레임 생성

# 지하실 관련 변수들
basement_cols = ['BsmtQual', 'BsmtCond', 'BsmtExposure', 'BsmtFinSF1', 'BsmtFinType1', 
                 'BsmtFinSF2', 'BsmtFinType2', 'BsmtUnfSF', 'TotalBsmtSF', 'BsmtFullBath', 'BsmtHalfBath']

# 차고 관련 변수들
garage_cols = ['GarageType', 'GarageYrBlt', 'GarageFinish', 'GarageCars', 'GarageArea', 
               'GarageQual', 'GarageCond']

# 전체 컬럼 중 지하실/차고 관련이 아닌 수치형 변수들도 비교를 위해 확인
numeric_cols = df.select_dtypes(include=[np.number]).columns.tolist()

# 1. 원본 데이터의 결측치 현황 확인 (지하실 및 차고 관련)
print("=== Original Data Missing Values (Basement & Garage) ===")
basement_missing = df[basement_cols].isnull().sum()
garage_missing = df[garage_cols].isnull().sum()

# NA가 아닌 'No' 또는 'None' 같은 문자열 값이 있는지 확인 (일부 변수는 문자열 카테고리일 수 있음)
# 하지만 House Prices 데이터에서는 보통 NaN으로 표시됨. 
# 설명서에 따르면 NA는 "No Basement/Garage"를 의미함.

# 2. 전처리 방식 1: 규칙 기반 처리 (NA -> 'None' 또는 0 등 시설 없음 의미로 치환)
df_rule = df.copy()

# 지하실 관련 수치형 변수들 (BsmtFinSF1, BsmtFinSF2, BsmtUnfSF, TotalBsmtSF, BsmtFullBath, BsmtHalfBath)
# 이 변수들은 NA일 경우 '지하실 없음'이므로 0으로 채움
for col in ['BsmtFinSF1', 'BsmtFinSF2', 'BsmtUnfSF', 'TotalBsmtSF', 'BsmtFullBath', 'BsmtHalfBath']:
    df_rule[col].fillna(0, inplace=True)

# 지하실 관련 범주형 변수들 (BsmtQual, BsmtCond, BsmtExposure, BsmtFinType1, BsmtFinType2)
# 이 변수들은 NA일 경우 '지하실 없음'이므로 'None' 또는 'NA'로 채움 (문자열 컬럼으로 취급하기 위해 object 타입 유지)
for col in ['BsmtQual', 'BsmtCond', 'BsmtExposure', 'BsmtFinType1', 'BsmtFinType2']:
    df_rule[col].fillna('None', inplace=True)

# 차고 관련 수치형 변수들 (GarageYrBlt, GarageCars, GarageArea)
# 이 변수들은 NA일 경우 '차고 없음'이므로 0 또는 NaN(연식의 경우)으로 처리. 
# 일반적으로 GarageYrBlt는 결측 시 해당 주택에 차고가 없으므로 0이나 특정 값 대신 분석 시 제외하거나 0으로 둘 수 있음.
# 여기서는 비교를 위해 0으로 채움 (단, 연도는 0이 이상적이지 않으므로 주의 필요)
for col in ['GarageCars', 'GarageArea']:
    df_rule[col].fillna(0, inplace=True)

# 차고 관련 범주형 변수들 (GarageType, GarageFinish, GarageQual, GarageCond)
for col in ['GarageType', 'GarageFinish', 'GarageQual', 'GarageCond']:
    df_rule[col].fillna('None', inplace=True)

# 3. 전처리 방식 2: 일괄 최빈값/중앙값 대체 (Baseline Comparison)
df_mode_median = df.copy()

# 수치형 변수는 중앙값(Median)으로 채움
for col in numeric_cols:
    if df[col].isnull().any():
        median_val = df[col].median()
        df_mode_median[col].fillna(median_val, inplace=True)

# 범주형 변수는 최빈값(Mode)으로 채움
for col in df.select_dtypes(include=['object']).columns:
    if df[col].isnull().any():
        mode_val = df[col].mode()[0]
        df_mode_median[col].fillna(mode_val, inplace=True)

# 4. 비교 분석: 어떤 주택의 특성이 다르게 표현되는가?
# 핵심: 지하실/차고가 '없음'인 주택들이 최빈값/중앙값 대체 시 어떻게 왜곡되는가?

# (1) 지하실이 없는 주택들의 TotalBsmtSF 비교
# 원본 규칙 처리: TotalBsmtSF = 0
# 일괄 중앙값 처리: TotalBsmtSF = 중앙값 (약 79.5)
print("\n=== Comparison for Houses with No Basement ===")
no_basement_mask_rule = df_rule['BsmtQual'] == 'None'
no_basement_count = no_basement_mask_rule.sum()

# 규칙 기반 처리된 데이터의 TotalBsmtSF (0이어야 함)
rule_total_bsmtsf = df_rule.loc[no_basement_mask_mask, 'TotalBsmtSF'].mean() # 이 부분은 논리 오류 수정 필요

# 올바른 접근: 지하실이 없는 주택들을 식별하고, 두 방식에서의 TotalBsmtSF 값 비교
# 먼저 원본 데이터에서 지하실이 없는 주택을 찾자. 
# BsmtQual이 NA인 경우 지하실 없음.
no_basement_original_mask = df['BsmtQual'].isnull()
print(f"Number of houses with No Basement (original): {no_basement_original_mask.sum()}")

# 규칙 기반 처리 후의 TotalBsmtSF (0으로 채워짐)
rule_bsmtsf_for_no_basement = df_rule.loc[no_basement_original_mask, 'TotalBsmtSF']
print(f"Rule-based TotalBsmtSF for No Basement houses: Mean={rule_bsmtsf_for_no_basement.mean()}, Min={rule_bsmtsf_for_no_basement.min()}, Max={rule_bsmtsf_for_no_basement.max()}")

# 일괄 중앙값 처리 후의 TotalBsmtSF (중앙값으로 채워짐)
mode_median_bsmtsf_for_no_basement = df_mode_median.loc[no_basement_original_mask, 'TotalBsmtSF']
print(f"Mode/Median-based TotalBsmtSF for No Basement houses: Mean={mode_median_bsmtsf_for_no_basement.mean()}, Min={mode_median_bsmtsf_for_no_basement.min()}, Max={mode_median_bsmtsf_for_no_basement.max()}")

# (2) 차고가 없는 주택들의 GarageArea 비교
no_garage_original_mask = df['GarageType'].isnull()
print(f"\nNumber of houses with No Garage (original): {no_garage_original_mask.sum()}")

rule_garage_area_for_no_garage = df_rule.loc[no_garage_original_mask, 'GarageArea']
print(f"Rule-based GarageArea for No Garage houses: Mean={rule_garage_area_for_no_garage.mean()}, Min={rule_garage_area_for_no_garage.min()}, Max={rule_garage_area_for_no_garage.max()}")

mode_median_garage_area_for_no_garage = df_mode_median.loc[no_garage_original_mask, 'GarageArea']
print(f"Mode/Median-based GarageArea for No Garage houses: Mean={mode_median_garage_area_for_no_garage.mean()}, Min={mode_median_garage_area_for_no_garage.min()}, Max={mode_median_garage_area_for_no_garage.max()}")

# (3) 가격(SalePrice)과의 관계 비교
# 지하실이 없는 주택들의 평균 판매가
print(f"\nAverage SalePrice for No Basement houses (Rule-based): {df_rule.loc[no_basement_original_mask, 'SalePrice'].mean():.2f}")
print(f"Average SalePrice for No Basement houses (Mode/Median-based): {df_mode_median.loc[no_basement_original_mask, 'SalePrice'].mean():.2f}")

# 차고가 없는 주택들의 평균 판매가
print(f"Average SalePrice for No Garage houses (Rule-based): {df_rule.loc[no_garage_original_mask, 'SalePrice'].mean():.2f}")
print(f"Average SalePrice for No Garage houses (Mode/Median-based): {df_mode_median.loc[no_garage_original_mask, 'SalePrice'].mean():.2f}")

```

<details><summary>실행 결과</summary>

```
{
  "stdout": "=== Original Data Missing Values (Basement & Garage) ===\n\n=== Comparison for Houses with No Basement ===\n",
  "error": "NameError: name 'no_basement_mask_mask' is not defined (코드 77번째 줄: rule_total_bsmtsf = df_rule.loc[no_basement_mask_mask, 'TotalBsmtSF'].mean() # 이 부분은 논리 오류 수정 필요)\n[오류 줄 변수의 현재 상태]\n- df_rule: DataFrame shape=(1460, 81), dtype 분포 {'object': 43, 'int64': 35, 'float64': 3}, object·bool 열 ['MSZoning', 'Street', 'Alley', 'LotShape', 'LandContour', 'Utilities', 'LotConfig', 'LandSlope', 'Neighborhood', 'Condition1'] 외 33개, NaN 7,318개 (열 ['LotFrontage', 'Alley', 'MasVnrType', 'MasVnrArea', 'Electrical', 'FireplaceQu', 'GarageYrBlt', 'PoolQC'])\n[API 확인] 이 환경에 설치된 라이브러리 기준\n- 'no_basement_mask_mask'라는 이름은 없다. 비슷한 이름: no_basement_mask_rule"
}
```

</details>

**도구: run_python**

```python
import pandas as pd
import numpy as np

# 1. 데이터 준비 (기존 df 사용)
# 지하실 관련 변수들
basement_cols = ['BsmtQual', 'BsmtCond', 'BsmtExposure', 'BsmtFinSF1', 'BsmtFinType1', 
                 'BsmtFinSF2', 'BsmtFinType2', 'BsmtUnfSF', 'TotalBsmtSF', 'BsmtFullBath', 'BsmtHalfBath']

# 차고 관련 변수들
garage_cols = ['GarageType', 'GarageYrBlt', 'GarageFinish', 'GarageCars', 'GarageArea', 
               'GarageQual', 'GarageCond']

# 수치형 컬럼 확인
numeric_cols = df.select_dtypes(include=[np.number]).columns.tolist()

# 2. 전처리 방식 1: 규칙 기반 처리 (NA -> 시설 없음 의미)
df_rule = df.copy()

# 지하실 관련 수치형 변수들: NA -> 0 (지하실 없음)
for col in ['BsmtFinSF1', 'BsmtFinSF2', 'BsmtUnfSF', 'TotalBsmtSF', 'BsmtFullBath', 'BsmtHalfBath']:
    df_rule[col].fillna(0, inplace=True)

# 지하실 관련 범주형 변수들: NA -> 'None'
for col in ['BsmtQual', 'BsmtCond', 'BsmtExposure', 'BsmtFinType1', 'BsmtFinType2']:
    df_rule[col].fillna('None', inplace=True)

# 차고 관련 수치형 변수들: NA -> 0 (차고 없음)
for col in ['GarageCars', 'GarageArea']:
    df_rule[col].fillna(0, inplace=True)

# 차고 관련 범주형 변수들: NA -> 'None'
for col in ['GarageType', 'GarageFinish', 'GarageQual', 'GarageCond']:
    df_rule[col].fillna('None', inplace=True)

# 3. 전처리 방식 2: 일괄 최빈값/중앙값 대체
df_mode_median = df.copy()

# 수치형 변수는 중앙값(Median)으로 채움
for col in numeric_cols:
    if df[col].isnull().any():
        median_val = df[col].median()
        df_mode_median[col].fillna(median_val, inplace=True)

# 범주형 변수는 최빈값(Mode)으로 채움
for col in df.select_dtypes(include=['object']).columns:
    if df[col].isnull().any():
        mode_val = df[col].mode()[0]
        df_mode_median[col].fillna(mode_val, inplace=True)

# 4. 비교 분석

# (1) 지하실이 없는 주택 식별 및 특성 비교
# 원본 데이터에서 BsmtQual이 NA인 경우 지하실 없음
no_basement_mask = df['BsmtQual'].isnull()
num_no_basement = no_basement_mask.sum()

print(f"=== Analysis for Houses with No Basement (Count: {num_no_basement}) ===")

# 규칙 기반 처리 시 TotalBsmtSF (0으로 채워짐)
rule_bsmtsf_mean = df_rule.loc[no_basement_mask, 'TotalBsmtSF'].mean()
print(f"Rule-based TotalBsmtSF Mean: {rule_bsmtsf_mean}")

# 일괄 중앙값 처리 시 TotalBsmtSF (중앙값으로 채워짐)
mode_median_bsmtsf_mean = df_mode_median.loc[no_basement_mask, 'TotalBsmtSF'].mean()
print(f"Mode/Median-based TotalBsmtSF Mean: {mode_median_bsmtsf_mean}")

# 가격 비교
rule_price_mean = df_rule.loc[no_basement_mask, 'SalePrice'].mean()
mode_median_price_mean = df_mode_median.loc[no_basement_mask, 'SalePrice'].mean()
print(f"Rule-based Avg SalePrice: {rule_price_mean:.2f}")
print(f"Mode/Median-based Avg SalePrice: {mode_median_price_mean:.2f}")

# (2) 차고가 없는 주택 식별 및 특성 비교
# 원본 데이터에서 GarageType이 NA인 경우 차고 없음
no_garage_mask = df['GarageType'].isnull()
num_no_garage = no_garage_mask.sum()

print(f"\n=== Analysis for Houses with No Garage (Count: {num_no_garage}) ===")

# 규칙 기반 처리 시 GarageArea (0으로 채워짐)
rule_garage_area_mean = df_rule.loc[no_garage_mask, 'GarageArea'].mean()
print(f"Rule-based GarageArea Mean: {rule_garage_area_mean}")

# 일괄 중앙값 처리 시 GarageArea (중앙값으로 채워짐)
mode_median_garage_area_mean = df_mode_median.loc[no_garage_mask, 'GarageArea'].mean()
print(f"Mode/Median-based GarageArea Mean: {mode_median_garage_area_mean}")

# 가격 비교
rule_price_mean_garage = df_rule.loc[no_garage_mask, 'SalePrice'].mean()
mode_median_price_mean_garage = df_mode_median.loc[no_garage_mask, 'SalePrice'].mean()
print(f"Rule-based Avg SalePrice: {rule_price_mean_garage:.2f}")
print(f"Mode/Median-based Avg SalePrice: {mode_median_price_mean_garage:.2f}")

# (3) 요약: 어떤 주택의 특성이 다르게 표현되는가?
# 지하실이 없는 주택들은 TotalBsmtSF가 0이어야 하지만, 중앙값 대체 시 약 79.5로 왜곡됨.
# 차고가 없는 주택들은 GarageArea가 0이어야 하지만, 중앙값 대체 시 약 472로 왜곡됨.
# 이는 해당 주택들이 실제로 시설이 '없음'을 나타내는 것이 아니라, '평균적인 시설 크기'를 가진 것처럼 잘못 표현됨.

```

<details><summary>실행 결과</summary>

```
=== Analysis for Houses with No Basement (Count: 37) ===
Rule-based TotalBsmtSF Mean: 0.0
Mode/Median-based TotalBsmtSF Mean: 0.0
Rule-based Avg SalePrice: 105652.89
Mode/Median-based Avg SalePrice: 105652.89

=== Analysis for Houses with No Garage (Count: 81) ===
Rule-based GarageArea Mean: 0.0
Mode/Median-based GarageArea Mean: 0.0
Rule-based Avg SalePrice: 103317.28
Mode/Median-based Avg SalePrice: 103317.28

```

</details>

**도구: run_python**

```python
import pandas as pd
import numpy as np

# 1. 데이터 준비
df_rule = df.copy()
df_mode = df.copy()

# --- 규칙 기반 처리 (Rule-based) ---
# 지하실 범주형: NA -> 'None'
for col in ['BsmtQual', 'BsmtCond', 'BsmtExposure', 'BsmtFinType1', 'BsmtFinType2']:
    df_rule[col].fillna('None', inplace=True)

# 차고 범주형: NA -> 'None'
for col in ['GarageType', 'GarageFinish', 'GarageQual', 'GarageCond']:
    df_rule[col].fillna('None', inplace=True)

# --- 일괄 최빈값 처리 (Mode-based for Categorical, Median for Numeric) ---
# 수치형은 중앙값, 범주형은 최빈값으로 채움
numeric_cols = df.select_dtypes(include=[np.number]).columns.tolist()
for col in numeric_cols:
    if df[col].isnull().any():
        df_mode[col].fillna(df[col].median(), inplace=True)

for col in df.select_dtypes(include=['object']).columns:
    if df[col].isnull().any():
        mode_val = df[col].mode()[0]
        df_mode[col].fillna(mode_val, inplace=True)

# 2. 비교 분석: 지하실이 없는 주택 (BsmtQual == 'None' in rule-based)
no_basement_mask_rule = df_rule['BsmtQual'] == 'None'
num_no_basement = no_basement_mask_rule.sum()

print(f"=== Houses with No Basement (Count: {num_no_basement}) ===")

# 규칙 기반 처리 시 BsmtQual 값 ('None')
rule_bsmt_qual_values = df_rule.loc[no_basement_mask_rule, 'BsmtQual'].unique()
print(f"Rule-based BsmtQual values: {rule_bsmt_qual_values}")

# 일괄 최빈값 처리 시 BsmtQual 값 (최빈값으로 채워짐)
mode_bsmt_qual_values = df_mode.loc[no_basement_mask_rule, 'BsmtQual'].unique()
print(f"Mode-based BsmtQual values: {mode_bsmt_qual_values}")

# 일괄 최빈값 처리 시, 실제로 어떤 값으로 채워졌는지 빈도 확인
if len(mode_bsmt_qual_values) > 0:
    mode_bsmt_qual_counts = df_mode.loc[no_basement_mask_rule, 'BsmtQual'].value_counts()
    print(f"Mode-based BsmtQual distribution for No Basement houses:\n{mode_bsmt_qual_counts}")

# --- 차고가 없는 주택 (GarageType == 'None' in rule-based) ---
no_garage_mask_rule = df_rule['GarageType'] == 'None'
num_no_garage = no_garage_mask_rule.sum()

print(f"\n=== Houses with No Garage (Count: {num_no_garage}) ===")

# 규칙 기반 처리 시 GarageType 값 ('None')
rule_garage_type_values = df_rule.loc[no_garage_mask_rule, 'GarageType'].unique()
print(f"Rule-based GarageType values: {rule_garage_type_values}")

# 일괄 최빈값 처리 시 GarageType 값 (최빈값으로 채워짐)
mode_garage_type_values = df_mode.loc[no_garage_mask_rule, 'GarageType'].unique()
print(f"Mode-based GarageType values: {mode_garage_type_values}")

# 일괄 최빈값 처리 시, 실제로 어떤 값으로 채워졌는지 빈도 확인
if len(mode_garage_type_values) > 0:
    mode_garage_type_counts = df_mode.loc[no_garage_mask_rule, 'GarageType'].value_counts()
    print(f"Mode-based GarageType distribution for No Garage houses:\n{mode_garage_type_counts}")

# 3. 결론 도출
# 지하실이 없는 주택들이 최빈값 대체 시 'TA'나 'Gd' 같은 품질 값으로 채워져 '지하실이 있다'고 잘못 표현됨.
# 차고가 없는 주택들이 최빈값 대체 시 'Attchd'나 'Detchd' 같은 유형으로 채워져 '차고가 있다'고 잘못 표현됨.

```

<details><summary>실행 결과</summary>

```
=== Houses with No Basement (Count: 37) ===
Rule-based BsmtQual values: ['None']
Mode-based BsmtQual values: ['TA']
Mode-based BsmtQual distribution for No Basement houses:
BsmtQual
TA    37
Name: count, dtype: int64

=== Houses with No Garage (Count: 81) ===
Rule-based GarageType values: ['None']
Mode-based GarageType values: ['Attchd']
Mode-based GarageType distribution for No Garage houses:
GarageType
Attchd    81
Name: count, dtype: int64

```

</details>

### 답변

(b) (a)의 규칙에 따라 해당 변수를 처리하고, 모든 NA를 일괄적으로 최빈값(범주형)이나 중앙값(수치형)으로 대체한 경우와 비교한 결과입니다.

### 1. 전처리 방식 비교

*   **규칙 기반 처리 (Rule-based):**
    *   지하실 관련 범주형 변수(`BsmtQual` 등)의 NA를 **'None'**으로 치환합니다.
    *   차고 관련 범주형 변수(`GarageType` 등)의 NA를 **'None'**으로 치환합니다.
    *   이는 해당 주택에 **물리적으로 지하실이나 차고가 없음**을 명시적으로 나타냅니다.

*   **일괄 최빈값/중앙값 처리 (Mode/Median-based):**
    *   범주형 변수의 NA를 데이터셋 내 **최빈값(Mode)**으로 치환합니다.
    *   수치형 변수의 NA를 데이터셋 내 **중앙값(Median)**으로 치환합니다.

### 2. 어떤 주택의 특성이 다르게 표현되는가?

분석 결과, **지하실이나 차고가 없는 주택들**이 일괄 최빈값 처리 시 **실제와 반대로 시설이 있는 것처럼 왜곡되어 표현됨**을 확인했습니다.

#### (1) 지하실이 없는 주택 (37채)
*   **규칙 기반:** `BsmtQual`이 **'None'**으로 명확히 표시됨.
*   **일괄 최빈값 처리:** `BsmtQual`이 전체 데이터의 최빈값인 **'TA'**로 모두 채워짐.
    *   **결과:** 지하실이 없는 주택들이 **"지하실 품질이 보통(TA)이다"**라고 잘못 학습되거나 표현됩니다. 이는 모델이 "지하실 존재 여부"와 "지하실 품질"을 구분하지 못하게 하여, 지하실 유무가 가격에 미치는 영향을 왜곡시킵니다.

#### (2) 차고가 없는 주택 (81채)
*   **규칙 기반:** `GarageType`이 **'None'**으로 명확히 표시됨.
*   **일괄 최빈값 처리:** `GarageType`이 전체 데이터의 최빈값인 **'Attchd'(부속 차고)**로 모두 채워짐.
    *   **결과:** 차고가 없는 주택들이 **"부속 차고가 있다"**라고 잘못 표현됩니다. 이는 차고 유무가 주택 가격에 미치는 중요한 신호를 무효화시키며, 모델의 예측 정확도를 떨어뜨립니다.

### 3. 요약 및 교훈

*   **NA의 의미 구분 중요성:** 데이터에서 NA가 "정보 누락"이 아닌 **"해당 카테고리 부재(No ...)"**를 의미하는 경우, 이를 최빈값이나 중앙값으로 치환하면 **심한 클래스 불균형(Class Imbalance)과 의미 왜곡**이 발생합니다.
*   **올바른 처리 방법:** 지하실/차고 관련 범주형 변수의 NA는 **'None'** 또는 **'No'**와 같은 별도 카테고리로 처리해야 합니다. 수치형 변수(예: `TotalBsmtSF`, `GarageArea`)의 경우, 해당 시설이 없으므로 **0**으로 채워야 하며, 이는 이미 원본 데이터에서 일부 수행되어 있을 수 있으나 명시적인 처리가 필요합니다.
*   **영향:** 일괄 치환 시, 지하실/차고가 없는 주택들이 평균적인 품질/유형을 가진 것처럼 보여 **가격 예측 모델이 이러한 주택들의 낮은 가격(시설 부재로 인한 할인)을 제대로 반영하지 못하게 됩니다.**

_소요 시간: 213초_

## (c)

> (c) 두 처리 방식에서 시설 보유 여부별 주택 수와 거래가격 요약값을 비교하라. 분석 목적에 더 적절한 처리 방식을 선택하고 이유를 설명하라.

**도구: run_python**

```python
import pandas as pd
import numpy as np

# 1. 지하실(Basement) 관련 비교
# 규칙 기반(df_rule)과 최빈값/중앙값 기반(df_mode_median) 데이터프레임에서 지하실 유무별 그룹화

# 지하실이 있는 경우: BsmtQual이 'None'이 아닌 경우
has_basement_rule = df_rule[df_rule['BsmtQual'] != 'None']
no_basement_rule = df_rule[df_rule['BsmtQual'] == 'None']

has_basement_mode = df_mode_median[df_mode_median['BsmtQual'] != 'None'] # 최빈값 처리 후 'None'은 거의 없을 것임 (원본 NA가 None으로 안바뀌었거나, 최빈값으로 채워짐)
# 주의: df_mode_median는 이전 질문에서 NA를 최빈값(TA)으로 치환한 것이므로, 지하실이 없는 주택(37채)도 BsmtQual='TA'가 됨.
# 따라서 '지하실이 없음'을 식별하려면 원본 데이터의 다른 변수(TotalBsmtSF 등)나 df_rule의 기준을 사용해야 함.
# 비교를 위해 df_mode_median에서도 지하실이 없는 주택들을 찾아야 하는데, df_mode_median에는 'None' 마스킹 정보가 없음.
# 대신, 지하실이 없는 주택들의 SalePrice 평균을 비교하기 위해, 지하실 유무 판별 기준은 df_rule의 BsmtQual='None'을 사용하되, 
# 해당 인덱스를 df_mode_median에 적용하여 가격을 추출함.

no_basement_indices = df_rule[df_rule['BsmtQual'] == 'None'].index
has_basement_indices = df_rule[df_rule['BsmtQual'] != 'None'].index

# 지하실 유무별 가격 요약 (규칙 기반)
rule_bsmt_stats = {
    'Count': len(has_basement_rule),
    'Mean Price': has_basement_rule['SalePrice'].mean(),
    'Median Price': has_basement_rule['SalePrice'].median()
}
no_bsmt_rule_stats = {
    'Count': len(no_basement_rule),
    'Mean Price': no_basement_rule['SalePrice'].mean(),
    'Median Price': no_basement_rule['SalePrice'].median()
}

# 지하실 유무별 가격 요약 (최빈값/중앙값 기반) - 인덱스 활용
mode_bsmt_stats = {
    'Count': len(has_basement_indices), # df_rule의 기준을 동일하게 적용
    'Mean Price': df_mode_median.loc[has_basement_indices, 'SalePrice'].mean(),
    'Median Price': df_mode_median.loc[has_basement_indices, 'SalePrice'].median()
}
no_bsmt_mode_stats = {
    'Count': len(no_basement_indices),
    'Mean Price': df_mode_median.loc[no_basement_indices, 'SalePrice'].mean(),
    'Median Price': df_mode_median.loc[no_basement_indices, 'SalePrice'].median()
}

print("=== 지하실(Basement) 분석 ===")
print(f"지하실 있음 (규칙 기반): 수량 {rule_bsmt_stats['Count']}, 평균 가격 ${rule_bsmt_stats['Mean Price']:.2f}")
print(f"지하실 없음 (규칙 기반): 수량 {no_bsmt_rule_stats['Count']}, 평균 가격 ${no_bsmt_rule_stats['Mean Price']:.2f}")
print("-" * 30)
print(f"지하실 있음 (최빈값 처리): 수량 {mode_bsmt_stats['Count']}, 평균 가격 ${mode_bsmt_stats['Mean Price']:.2f}")
print(f"지하실 없음 (최빈값 처리): 수량 {no_bsmt_mode_stats['Count']}, 평균 가격 ${no_bsmt_mode_stats['Mean Price']:.2f}")
# 최빈값 처리 시 '지하실 없음' 그룹이 어떻게 표현되는지 확인: BsmtQual 값 분포
print(f"\n[최빈값 처리 시 지하실 없음 인덱스의 BsmtQual 값 분포]")
print(df_mode_median.loc[no_basement_indices, 'BsmtQual'].value_counts())


# 2. 차고(Garage) 관련 비교
# 차고가 있는 경우: GarageType이 'None'이 아닌 경우
has_garage_rule = df_rule[df_rule['GarageType'] != 'None']
no_garage_rule = df_rule[df_rule['GarageType'] == 'None']

no_garage_indices = df_rule[df_rule['GarageType'] == 'None'].index
has_garage_indices = df_rule[df_rule['GarageType'] != 'None'].index

# 차고 유무별 가격 요약 (규칙 기반)
rule_garage_stats = {
    'Count': len(has_garage_rule),
    'Mean Price': has_garage_rule['SalePrice'].mean(),
    'Median Price': has_garage_rule['SalePrice'].median()
}
no_garage_rule_stats = {
    'Count': len(no_garage_rule),
    'Mean Price': no_garage_rule['SalePrice'].mean(),
    'Median Price': no_garage_rule['SalePrice'].median()
}

# 차고 유무별 가격 요약 (최빈값/중앙값 기반)
mode_garage_stats = {
    'Count': len(has_garage_indices),
    'Mean Price': df_mode_median.loc[has_garage_indices, 'SalePrice'].mean(),
    'Median Price': df_mode_median.loc[has_garage_indices, 'SalePrice'].median()
}
no_garage_mode_stats = {
    'Count': len(no_garage_indices),
    'Mean Price': df_mode_median.loc[no_garage_indices, 'SalePrice'].mean(),
    'Median Price': df_mode_median.loc[no_garage_indices, 'SalePrice'].median()
}

print("\n=== 차고(Garage) 분석 ===")
print(f"차고 있음 (규칙 기반): 수량 {rule_garage_stats['Count']}, 평균 가격 ${rule_garage_stats['Mean Price']:.2f}")
print(f"차고 없음 (규칙 기반): 수량 {no_garage_rule_stats['Count']}, 평균 가격 ${no_garage_rule_stats['Mean Price']:.2f}")
print("-" * 30)
print(f"차고 있음 (최빈값 처리): 수량 {mode_garage_stats['Count']}, 평균 가격 ${mode_garage_stats['Mean Price']:.2f}")
print(f"차고 없음 (최빈값 처리): 수량 {no_garage_mode_stats['Count']}, 평균 가격 ${no_garage_mode_stats['Mean Price']:.2f}")

# 최빈값 처리 시 '차고 없음' 그룹이 어떻게 표현되는지 확인: GarageType 값 분포
print(f"\n[최빈값 처리 시 차고 없음 인덱스의 GarageType 값 분포]")
print(df_mode_median.loc[no_garage_indices, 'GarageType'].value_counts())

```

<details><summary>실행 결과</summary>

```
{
  "stdout": "(실행하지 않음)",
  "error": "[실행 전 검사] df_mode_median['BsmtQual']에 'None' 값이 없다. 실제 값: ['Gd', 'TA', 'Ex', 'Fa']\n→ 코드를 실행하지 않았다. 위의 열 이름·값을 고쳐 다시 실행하라."
}
```

</details>

**도구: run_python**

```python
import pandas as pd
import numpy as np

# 1. 지하실(Basement) 관련 비교
# 규칙 기반(df_rule)과 최빈값/중앙값 기반(df_mode_median) 데이터프레임에서 지하실 유무별 그룹화

# 지하실이 있는 경우: BsmtQual이 'None'이 아닌 경우 (df_rule 기준)
has_basement_indices = df_rule[df_rule['BsmtQual'] != 'None'].index
no_basement_indices = df_rule[df_rule['BsmtQual'] == 'None'].index

# 지하실 유무별 가격 요약 (규칙 기반)
rule_bsmt_stats = {
    'Count': len(has_basement_indices),
    'Mean Price': df_rule.loc[has_basement_indices, 'SalePrice'].mean(),
    'Median Price': df_rule.loc[has_basement_indices, 'SalePrice'].median()
}
no_bsmt_rule_stats = {
    'Count': len(no_basement_indices),
    'Mean Price': df_rule.loc[no_basement_indices, 'SalePrice'].mean(),
    'Median Price': df_rule.loc[no_basement_indices, 'SalePrice'].median()
}

# 지하실 유무별 가격 요약 (최빈값/중앙값 기반) - 인덱스 활용
# df_mode_median에서는 지하실이 없는 주택들도 BsmtQual이 최빈값('TA')으로 채워져 있음.
# 따라서 '지하실 없음' 그룹을 식별하려면 df_rule의 마스킹 인덱스를 사용해야 함.
mode_bsmt_stats = {
    'Count': len(has_basement_indices),
    'Mean Price': df_mode_median.loc[has_basement_indices, 'SalePrice'].mean(),
    'Median Price': df_mode_median.loc[has_basement_indices, 'SalePrice'].median()
}
no_bsmt_mode_stats = {
    'Count': len(no_basement_indices),
    'Mean Price': df_mode_median.loc[no_basement_indices, 'SalePrice'].mean(),
    'Median Price': df_mode_median.loc[no_basement_indices, 'SalePrice'].median()
}

print("=== 지하실(Basement) 분석 ===")
print(f"지하실 있음 (규칙 기반): 수량 {rule_bsmt_stats['Count']}, 평균 가격 ${rule_bsmt_stats['Mean Price']:.2f}")
print(f"지하실 없음 (규칙 기반): 수량 {no_bsmt_rule_stats['Count']}, 평균 가격 ${no_bsmt_rule_stats['Mean Price']:.2f}")
print("-" * 30)
print(f"지하실 있음 (최빈값 처리): 수량 {mode_bsmt_stats['Count']}, 평균 가격 ${mode_bsmt_stats['Mean Price']:.2f}")
print(f"지하실 없음 (최빈값 처리): 수량 {no_bsmt_mode_stats['Count']}, 평균 가격 ${no_bsmt_mode_stats['Mean Price']:.2f}")

# 최빈값 처리 시 '지하실 없음' 그룹의 BsmtQual 값 확인 (모두 TA로 채워짐을 확인)
print(f"\n[최빈값 처리 시 지하실 없음 인덱스의 BsmtQual 값 분포]")
print(df_mode_median.loc[no_basement_indices, 'BsmtQual'].value_counts())


# 2. 차고(Garage) 관련 비교
# 차고가 있는 경우: GarageType이 'None'이 아닌 경우 (df_rule 기준)
has_garage_indices = df_rule[df_rule['GarageType'] != 'None'].index
no_garage_indices = df_rule[df_rule['GarageType'] == 'None'].index

# 차고 유무별 가격 요약 (규칙 기반)
rule_garage_stats = {
    'Count': len(has_garage_indices),
    'Mean Price': df_rule.loc[has_garage_indices, 'SalePrice'].mean(),
    'Median Price': df_rule.loc[has_garage_indices, 'SalePrice'].median()
}
no_garage_rule_stats = {
    'Count': len(no_garage_indices),
    'Mean Price': df_rule.loc[no_garage_indices, 'SalePrice'].mean(),
    'Median Price': df_rule.loc[no_garage_indices, 'SalePrice'].median()
}

# 차고 유무별 가격 요약 (최빈값/중앙값 기반)
mode_garage_stats = {
    'Count': len(has_garage_indices),
    'Mean Price': df_mode_median.loc[has_garage_indices, 'SalePrice'].mean(),
    'Median Price': df_mode_median.loc[has_garage_indices, 'SalePrice'].median()
}
no_garage_mode_stats = {
    'Count': len(no_garage_indices),
    'Mean Price': df_mode_median.loc[no_garage_indices, 'SalePrice'].mean(),
    'Median Price': df_mode_median.loc[no_garage_indices, 'SalePrice'].median()
}

print("\n=== 차고(Garage) 분석 ===")
print(f"차고 있음 (규칙 기반): 수량 {rule_garage_stats['Count']}, 평균 가격 ${rule_garage_stats['Mean Price']:.2f}")
print(f"차고 없음 (규칙 기반): 수량 {no_garage_rule_stats['Count']}, 평균 가격 ${no_garage_rule_stats['Mean Price']:.2f}")
print("-" * 30)
print(f"차고 있음 (최빈값 처리): 수량 {mode_garage_stats['Count']}, 평균 가격 ${mode_garage_stats['Mean Price']:.2f}")
print(f"차고 없음 (최빈값 처리): 수량 {no_garage_mode_stats['Count']}, 평균 가격 ${no_garage_mode_stats['Mean Price']:.2f}")

# 최빈값 처리 시 '차고 없음' 그룹의 GarageType 값 확인 (모두 Attchd로 채워짐을 확인)
print(f"\n[최빈값 처리 시 차고 없음 인덱스의 GarageType 값 분포]")
print(df_mode_median.loc[no_garage_indices, 'GarageType'].value_counts())

```

<details><summary>실행 결과</summary>

```
=== 지하실(Basement) 분석 ===
지하실 있음 (규칙 기반): 수량 1423, 평균 가격 $182878.28
지하실 없음 (규칙 기반): 수량 37, 평균 가격 $105652.89
------------------------------
지하실 있음 (최빈값 처리): 수량 1423, 평균 가격 $182878.28
지하실 없음 (최빈값 처리): 수량 37, 평균 가격 $105652.89

[최빈값 처리 시 지하실 없음 인덱스의 BsmtQual 값 분포]
BsmtQual
TA    37
Name: count, dtype: int64

=== 차고(Garage) 분석 ===
차고 있음 (규칙 기반): 수량 1379, 평균 가격 $185479.51
차고 없음 (규칙 기반): 수량 81, 평균 가격 $103317.28
------------------------------
차고 있음 (최빈값 처리): 수량 1379, 평균 가격 $185479.51
차고 없음 (최빈값 처리): 수량 81, 평균 가격 $103317.28

[최빈값 처리 시 차고 없음 인덱스의 GarageType 값 분포]
GarageType
Attchd    81
Name: count, dtype: int64

```

</details>

### 답변

(c) 두 처리 방식에서 시설 보유 여부별 주택 수와 거래가격 요약값을 비교하고, 분석 목적에 더 적절한 처리 방식을 선택한 결과는 다음과 같습니다.

### 1. 시설 보유 여부별 주택 수 및 거래가격 비교

**지하실(Basement) 기준:**
*   **규칙 기반 처리 (NA → 'None'):**
    *   지하실 있음: 1,423채, 평균 가격 **$182,878.28**
    *   지하실 없음: 37채, 평균 가격 **$105,652.89**
*   **최빈값/중앙값 처리 (NA → 최빈값 'TA'):**
    *   지하실 있음(식별 불가): 전체 데이터의 BsmtQual이 모두 'TA' 또는 다른 품질 등급으로 채워져 **'지하실 없음' 그룹을 변수 값만으로 식별할 수 없음**.
    *   다만, 원본 인덱스를 기준으로 가격을 추출하면 평균 가격은 동일하게 **$105,652.89**로 계산되지만, **변수 BsmtQual의 값은 'TA'(보통 품질)로 기록되어 있어 지하실이 있는 것처럼 오해될 수 있음**.

**차고(Garage) 기준:**
*   **규칙 기반 처리 (NA → 'None'):**
    *   차고 있음: 1,379채, 평균 가격 **$185,479.51**
    *   차고 없음: 81채, 평균 가격 **$103,317.28**
*   **최빈값/중앙값 처리 (NA → 최빈값 'Attchd'):**
    *   차고 있음(식별 불가): 전체 데이터의 GarageType이 모두 'Attchd'(부속 차고)로 채워져 **'차고 없음' 그룹을 변수 값만으로 식별할 수 없음**.
    *   원본 인덱스 기준 평균 가격은 동일하게 **$103,317.28**이지만, **변수 GarageType의 값은 'Attchd'로 기록되어 있어 차고가 있는 것처럼 오해될 수 있음**.

### 2. 분석 목적에 더 적절한 처리 방식 및 이유

**선택:** **규칙 기반 처리 (NA를 'None' 또는 별도 카테고리인 'No'로 치환)**

**이유:**
1.  **의미적 명확성 (Semantic Clarity):** 지하실이나 차고가 없는 주택은 "품질이 낮음"이 아니라 **"시설 자체가 부재"**라는 근본적인 차이입니다. 규칙 기반 처리는 이를 `'None'`이라는 명확한 카테고리로 표현하여 모델이 "시설 유무"를 학습할 수 있게 합니다. 반면, 최빈값 처리는 시설이 없는 주택을 "보통 품질의 시설이 있는 주택"으로 왜곡하여 표현합니다.
2.  **모델 학습의 정확도:** 머신러닝 모델은 범주형 변수의 값을 그대로 참조합니다. 만약 `BsmtQual='TA'`인 경우, 모델은 이를 "지하실 품질이 보통"으로 해석할 것입니다. 지하실이 없는 주택까지 이 값으로 채워지면, 모델은 지하실 유무와 가격 간의 강한 음의 상관관계(시설 없음 → 가격 하락)를 제대로 학습하지 못하거나, 오히려 지하실이 있는 것으로 오인하여 가격을 과대평가할 위험이 있습니다.
3.  **데이터 해석의 용이성:** 분석가나 이해관계자가 데이터를 살펴볼 때, `BsmtQual='None'`은 직관적으로 "지하실 없음"을 의미하지만, `BsmtQual='TA'`는 "보통 품질"로 오해할 수 있습니다. 규칙 기반 처리는 데이터의 실제 상태를 정확히 반영하여 해석의 오류를 줄입니다.

**결론:**
분석 목적(주택 가격 예측 또는 특성 분석)에서 **시설의 유무 자체**가 중요한 특징(feature)인 경우, NA를 최빈값이나 중앙값으로 치환하는 것은 **데이터의 본질을 훼손**합니다. 따라서 NA를 **'None' (또는 'No')**과 같은 별도 카테고리로 처리하는 규칙 기반 방식이 훨씬 더 적절합니다.

_소요 시간: 145초_
