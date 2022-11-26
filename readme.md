# Mint Condition

Documenting Mint Query Syntax with the goal of creating a nicer UI, but looks pretty locked down at the moment

## Query Syntax

[![query syntax](https://i.imgur.com/Hh9BDnF.png)](http://s3.amazonaws.com/satisfaction-production/s3_images/200138/Transaction_Search_-_Engineering_-_Mint___Confluence.png)


| Syntax            | Description                                                                                                           | Example                      |
| ----------------- | --------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| `,`               | Separator of search terms. Terms of different types are ANDed together while terms of the same type are ORed together | Starbucks, category: Travel  |
| `-`               | Exclusion (Not). Exclusion takes precedence over everything else                                                      | -Apple , -category: Shopping |
| `"Exact, String"` | Double-quote strings that contain commas (or else they will be considered separate search terms)                      | "Apple, "Inc"                |
| `category=:`      | All transactions in the specified category (does not include subcategories). Name must be an exact match              | category=: Travel            |
| `category:`       | All transactions in the specified category and subcategories. Name must bean exact match                              | category: Travel             |
| `description:`    | All transactions with the specified description. Description must be an exact match                                   | description: Walmart         |
| `merchant:`       | Alias for `description`. Only for backwards compatibility                                                             | merchant: Walmart            |
| `tag:`            | All transactions with the specified tag. Name must be an exact match                                                  | tag: Taxable                 |
| `xx.y`            | All transactions with between Amount xx.y and xx.(Y+1 )                                                               | 32.0 shows [32.00, 32.09]    |
| `..n1`            | All transactions less than n1                                                                                         | ..31, ..32.0                 |
| `n1..`            | All transactions greater than n1                                                                                      | 31.., 32.0..                 |
| `n1..n2`          | All transactions greater than n1 but less than n2                                                                     | 31..42, 1...100              |

## Links

* [More transaction search/filter options (by amount range, etc)](https://web.archive.org/web/20120706044402/https://satisfaction.mint.com/mint/topics/more_transaction_search_filter_options_by_amount_range_etc)
* [Advanced Transaction Search](https://web.archive.org/web/20140303000400/https://satisfaction.mint.com/mint/topics/advanced_transaction_search)
* [3 Mint.com URL Hacks for Transactions](https://medium.com/@blitzten/manually-filtering-mint-coms-transactions-2d8ca5e1fa4)
* [View transactions within a specific date range in Mint](https://scottpdawson.com/view-transactions-within-date-range-in-mint/)

## Support

* [How can I view transactions within a specific date range?](https://mint.intuit.com/support/en-us/help-article/bank-transactions/view-transactions-within-specific-date-range/L7adKFpUf_US_en_US)

## Post Payload

```json
{
  "limit": 50,
  "offset": 0,
  "searchFilters": [
    {
      "matchAll": true,
      "filters": [
        {
          "type": "CategoryIdFilter",
          "categoryId": "47089146_1721568",
          "includeChildCategories": true
        },
        {
          "type": "DescriptionNameFilter",
          "description": "BEST BUY      00003608",
          "exclude": false
        },
        {
          "type": "TagIdFilter",
          "tagId": "47089146_268496"
        },
        {
          "type": "TransactionLiteralFilter",
          "searchFilter": "Best Buy"
        }
      ]
    }
  ],
  "dateFilter": {
    "type": "CUSTOM",
    "endDate": "2022-09-30",
    "startDate": "2022-09-01"
  },
  "sort": "DATE_DESCENDING"
}
```

## Get Params

```ini
?categoryIds=47089146_1721568
&startDate=2022-09-01
&endDate=2022-09-30
&exclHidden=T
```

```ini
https://mint.intuit.com/transaction?query=category:"restaurants"&startDate=06/01/2014&endDate=06/30/2014&exclHidden=T

https://wwws.mint.com/transaction.event
?query=category:%22restaurants%22
&startDate=06/01/2014
&endDate=06/30/2014
&exclHidden=T
```


### Deprecated

```none
https://wwws.mint.com/transaction.event#location:{"query":"category=: Fast Food, category=: Alcohol &amp; Bars, category=: Coffee Shops","offset":0,"typeFilter":"cash","typeSort":8}
https://wwws.mint.com/transaction.event#location:{"query":"-verified","typeFilter":"cash"}
```

