---
title: "Setup"
description: ""
lead: "The function used by the contract deployer to set up AMM parameters.
This must be called before users can interact with the contract."
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu:
  docs:
    parent: "Developer Guide"
weight: 34
toc: true
---


```
function setup(
    address _oracle,
    bytes32 _questionId,
    uint _numOutcomes,
    uint _subsidy,
    uint _overround
  ) public onlyOwner() {
    require(init == false,'Already init');
    require(_overround > 0,'Cannot have 0 overround');
    CT.prepareCondition(_oracle, _questionId, _numOutcomes);
    condition = CT.getConditionId(_oracle, _questionId, _numOutcomes);

    IERC20(token).safeTransferFrom(msg.sender, address(this), _subsidy);

    numOutcomes = _numOutcomes;
    int128 n = ABDKMath.fromUInt(_numOutcomes);
    int128 initial_subsidy = getTokenEth(token, _subsidy);

    int128 overround = ABDKMath.divu(_overround, 10000); //TODO: if the overround is too low, then the exp overflows
    alpha = ABDKMath.div(overround, ABDKMath.mul(n,ABDKMath.ln(n)));
    b = ABDKMath.mul(ABDKMath.mul(initial_subsidy, n), alpha);

    for(uint i=0; i<_numOutcomes; i++) {
      q.push(initial_subsidy);
    }

    init = true;

    total_shares = ABDKMath.mul(initial_subsidy, n);
    current_cost = cost();
  }
```
This function allows you to customise the parameters used by the market maker in employing the Ls LMSR algorithm.

This function can only be called by the contract deployer and it is necessary to be called before it is possible to trade with the market maker.

`_oracle`: this is the address that is required to report the outcome of the prediction market at resolution. It can either be an externally owned account or a reference to another smart contract. This does not need to be the same as the person that deployed the contract. There are a variety of approaches that can be used for choosing the oracle and we discuss these further below.

`_questionId`: This is a unique bytes32 representation of the question ID. This ID will be used by the oracle in order to report the outcome (and can therefore be used to allow a single oracle to report on multiple events). The question ID could be an ipfs hash to a file storing metadata for the prediction market (such as a description of each of the outcomes).

`_numOutcomes`: an integer representing the number of discrete outcomes available for the market.

`_subsidy`: this represents the total amount of tokens that will be used to fund the market maker and can be viewed as the initial liquidity. The properties of the LsLMSR are such that choosing large numbers for _subsidy (ie where you fund the market maker with large amounts of capital) provides a tighter spread for traders, however results in higher potential losses (bounded loss is proportional to initial capital)

`_overround`: this represents the amount of profit that can be made by the market maker or the 'houses edge'. It is expressed in 'bips' where a value of 100 is equivalent to 1% profit. For reference, traditional book makers use an overround of between 5 and 10%. It is worth noting here that the calculations used by LsLMSR involve exponential arithmetic. Where the overround value is small, it is possible for the calculation to overflow (this is a limitation of the ethereum virtual machine). The minimum overround is dependent on the number of outcomes for the market and is displayed in the table at the bottom of this page.

In order for the oracle to report the outcome of the prediction market, it must call the function `reportPayouts()` found on the conditional tokens contract. This can either be done directly where the oracle is an externally owned account, or by a separate smart contract.

It is possible to write additional smart contracts that can interact with chainlink nodes or kleros arbitration and parse this information in a manner that can be reported to the conditional tokens contract.

Through the clever use of smart contracts, any information that can be brought on-chain can be used to create a prediction market. 

# Minimum overround

|Number of outcomes | Minimum overround (bps) |
| --- | --- |
| 2 | 174 |
| 3 | 275 |
| 4 | 347 |
| 5 | 403 |
| 6 | 448 |
| 7 | 487 |
| 8 | 520 |
| 9 | 550 |
| 10 | 576 |
| 11 | 600 |
| 12 | 622 |
| 13 | 642 |
| 14 | 660 |
| 15 | 678 |
| 16 | 694 |
| 17 | 709 |
| 18 | 723 |
| 19 | 737 |
| 20 | 749 |
| 21 | 762 |
| 22 | 773 |
| 23 | 784 |
| 24 | 795 |
| 25 | 805 |
| 26 | 815 |
| 27 | 824 |
| 28 | 834 |
| 29 | 842 |
| 30 | 851 |
| 31 | 859 |
| 32 | 867 |
| 33 | 875 |
| 34 | 882 |
| 35 | 889 |
| 36 | 896 |
| 37 | 903 |
| 38 | 910 |
| 39 | 916 |
| 40 | 923 |
| 41 | 929 |
| 42 | 935 |
| 43 | 941 |
| 44 | 947 |
| 45 | 952 |
| 46 | 958 |
| 47 | 963 |
| 48 | 968 |
| 49 | 973 |
| 50 | 979 |
| 51 | 983 |
| 52 | 988 |
| 53 | 993 |
| 54 | 998 |
| 55 | 1002 |
| 56 | 1007 |
| 57 | 1011 |
| 58 | 1016 |
| 59 | 1020 |
| 60 | 1024 |
| 61 | 1028 |
| 62 | 1032 |
| 63 | 1036 |
| 64 | 1040 |
| 65 | 1044 |
| 66 | 1048 |
| 67 | 1052 |
| 68 | 1055 |
| 69 | 1059 |
| 70 | 1063 |
| 71 | 1066 |
| 72 | 1070 |
| 73 | 1073 |
| 74 | 1077 |
| 75 | 1080 |
| 76 | 1083 |
| 77 | 1086 |
| 78 | 1090 |
| 79 | 1093 |
| 80 | 1096 |
| 81 | 1099 |
| 82 | 1102 |
| 83 | 1105 |
| 84 | 1108 |
| 85 | 1111 |
| 86 | 1114 |
| 87 | 1117 |
| 88 | 1120 |
| 89 | 1123 |
| 90 | 1125 |
| 91 | 1128 |
| 92 | 1131 |
| 93 | 1134 |
| 94 | 1136 |
| 95 | 1139 |
| 96 | 1142 |
| 97 | 1144 |
| 98 | 1147 |
| 99 | 1149 |
| 100 | 1152 |
| 101 | 1154 |
| 102 | 1157 |
| 103 | 1159 |
| 104 | 1162 |
| 105 | 1164 |
| 106 | 1166 |
| 107 | 1169 |
| 108 | 1171 |
| 109 | 1173 |
| 110 | 1176 |
| 111 | 1178 |
| 112 | 1180 |
| 113 | 1182 |
| 114 | 1185 |
| 115 | 1187 |
| 116 | 1189 |
| 117 | 1191 |
| 118 | 1193 |
| 119 | 1195 |
| 120 | 1197 |
| 121 | 1199 |
| 122 | 1202 |
| 123 | 1204 |
| 124 | 1206 |
| 125 | 1208 |
| 126 | 1210 |
| 127 | 1212 |
| 128 | 1214 |
| 129 | 1215 |
| 130 | 1217 |
| 131 | 1219 |
| 132 | 1221 |
| 133 | 1223 |
| 134 | 1225 |
| 135 | 1227 |
| 136 | 1229 |
| 137 | 1230 |
| 138 | 1232 |
| 139 | 1234 |
| 140 | 1236 |
| 141 | 1238 |
| 142 | 1239 |
| 143 | 1241 |
| 144 | 1243 |
| 145 | 1245 |
| 146 | 1246 |
| 147 | 1248 |
| 148 | 1250 |
| 149 | 1251 |
| 150 | 1253 |
| 151 | 1255 |
| 152 | 1256 |
| 153 | 1258 |
| 154 | 1260 |
| 155 | 1261 |
| 156 | 1263 |
| 157 | 1265 |
| 158 | 1266 |
| 159 | 1268 |
| 160 | 1269 |
| 161 | 1271 |
| 162 | 1272 |
| 163 | 1274 |
| 164 | 1275 |
| 165 | 1277 |
| 166 | 1278 |
| 167 | 1280 |
| 168 | 1281 |
| 169 | 1283 |
| 170 | 1284 |
| 171 | 1286 |
| 172 | 1287 |
| 173 | 1289 |
| 174 | 1290 |
| 175 | 1292 |
| 176 | 1293 |
| 177 | 1295 |
| 178 | 1296 |
| 179 | 1297 |
| 180 | 1299 |
| 181 | 1300 |
| 182 | 1302 |
| 183 | 1303 |
| 184 | 1304 |
| 185 | 1306 |
| 186 | 1307 |
| 187 | 1308 |
| 188 | 1310 |
| 189 | 1311 |
| 190 | 1312 |
| 191 | 1314 |
| 192 | 1315 |
| 193 | 1316 |
| 194 | 1317 |
| 195 | 1319 |
| 196 | 1320 |
| 197 | 1321 |
| 198 | 1323 |
| 199 | 1324 |
| 200 | 1325 |
| 201 | 1326 |
| 202 | 1328 |
| 203 | 1329 |
| 204 | 1330 |
| 205 | 1331 |
| 206 | 1332 |
| 207 | 1334 |
| 208 | 1335 |
| 209 | 1336 |
| 210 | 1337 |
| 211 | 1338 |
| 212 | 1340 |
| 213 | 1341 |
| 214 | 1342 |
| 215 | 1343 |
| 216 | 1344 |
| 217 | 1345 |
| 218 | 1347 |
| 219 | 1348 |
| 220 | 1349 |
| 221 | 1350 |
| 222 | 1351 |
| 223 | 1352 |
| 224 | 1353 |
| 225 | 1355 |
| 226 | 1356 |
| 227 | 1357 |
| 228 | 1358 |
| 229 | 1359 |
| 230 | 1360 |
| 231 | 1361 |
| 232 | 1362 |
| 233 | 1363 |
| 234 | 1364 |
| 235 | 1365 |
| 236 | 1366 |
| 237 | 1368 |
| 238 | 1369 |
| 239 | 1370 |
| 240 | 1371 |
| 241 | 1372 |
| 242 | 1373 |
| 243 | 1374 |
| 244 | 1375 |
| 245 | 1376 |
| 246 | 1377 |
| 247 | 1378 |
| 248 | 1379 |
| 249 | 1380 |
| 250 | 1381 |
| 251 | 1382 |
| 252 | 1383 |
| 253 | 1384 |
| 254 | 1385 |
| 255 | 1386 |
