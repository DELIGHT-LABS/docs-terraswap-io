---
weight: 2
bookFlatSection: true
---

# Router - Classic

## Transaction

### Execute swap operations

Multi-hop swap operations via `native_swap`, `terra_swap`, `loop` and `astroport`<br />

Case 1) The first source token is a native token (KRT => UST => mABNB)<br />

```json
{
  "execute_swap_operations":{
      "operations":[
         {
            "native_swap":{
               "offer_denom":"ukrw",
               "ask_denom":"uusd"
            }
         },
         {
            "terra_swap":{
               "offer_asset_info":{
                  "native_token":{
                     "denom":"uusd"
                  }
               },
               "ask_asset_info":{
                  "token":{
                     "contract_addr":"terra1avryzxnsn2denq7p2d7ukm6nkck9s0rz2llgnc"
                  }
               }
            }
         }
      ],
      "minimum_receive":"88000"
   }
}
```

Case 2) The first source token is a CW20 token (ANC => UST => KRT)<br />

Note: `Binary()` means that this JSON message should be encoded into Base64.<br />

```json
{
  "send": {
    "amount": "100000000",
    "contract": "terra14z80rwpd0alzj4xdtgqdmcqt9wd9xj5ffd60wp",
    "msg": Binary({
        "execute_swap_operations":{
            "operations":[
                {
                    "terra_swap":{
                        "offer_asset_info":{
                            "token":{
                                "contract_addr":"terra1747mad58h0w4y589y3sk84r5efqdev9q4r02pc"
                            }
                        },
                        "ask_asset_info":{
                            "native_token":{
                                "denom":"uusd"
                            }
                        }
                    }
                },
                {
                    "native_swap":{
                        "offer_denom":"uusd",
                        "ask_denom":"ukrw"
                    }
                }
            ],
            "minimum_receive":"121917057920"
        }
    })
  }
}
```

## Query


### Simulate swap operations
Number of KRT that can be get through 10 mABNB <br>
```json
{
    "simulate_swap_operations":{
        "offer_amount":"10000000",
        "operations":[
            {
                "terra_swap":{
                    "offer_asset_info":{
                        "token":{
                            "contract_addr":"terra1747mad58h0w4y589y3sk84r5efqdev9q4r02pc"
                        }
                    },
                    "ask_asset_info":{
                        "native_token":{
                            "denom":"uusd"
                        }
                    }
                }
            },
            {
                "native_swap":{
                    "offer_denom":"uusd",
                    "ask_denom":"ukrw"
                }
            }
        ]
    }
}
```
ex) https://bombay-fcd.terra.dev/wasm/contracts/terra1c58wrdkyc0ynvvxcv834kz65nfsxmw2w0pwusq/store?query_msg={%22simulate_swap_operations%22:{%22offer_amount%22:%2210000000%22,%22operations%22:[{%22terra_swap%22:{%22offer_asset_info%22:{%22token%22:{%22contract_addr%22:%22terra1747mad58h0w4y589y3sk84r5efqdev9q4r02pc%22}},%22ask_asset_info%22:{%22native_token%22:{%22denom%22:%22uusd%22}}}},{%22native_swap%22:{%22offer_denom%22:%22uusd%22,%22ask_denom%22:%22ukrw%22}}]}}

### Reverse simulate swap operations
Number of ANC needed to get 10Luna <br>
** Native swap is not supported. <br>
```json
{
    "reverse_simulate_swap_operations":{
        "ask_amount":"10000000",
        "operations":[
            {
                "terra_swap":{
                    "offer_asset_info":{
                        "token":{
                            "contract_addr":"terra1747mad58h0w4y589y3sk84r5efqdev9q4r02pc"
                        }
                    },
                    "ask_asset_info":{
                        "native_token":{
                            "denom":"uusd"
                        }
                    }
                }
            },
            {
                "terra_swap":{
                    "offer_asset_info":{
                        "native_token":{
                            "denom":"uusd"
                        }
                    },
                    "ask_asset_info":{
                        "native_token":{
                            "denom":"uluna"
                        }
                    }
                }
            }
        ]
    }
}
```
ex) https://bombay-fcd.terra.dev/wasm/contracts/terra1c58wrdkyc0ynvvxcv834kz65nfsxmw2w0pwusq/store?query_msg={%22reverse_simulate_swap_operations%22:{%22ask_amount%22:%2210000000%22,%22operations%22:[{%22terra_swap%22:{%22offer_asset_info%22:{%22token%22:{%22contract_addr%22:%22terra1747mad58h0w4y589y3sk84r5efqdev9q4r02pc%22}},%22ask_asset_info%22:{%22native_token%22:{%22denom%22:%22uusd%22}}}},{%22terra_swap%22:{%22offer_asset_info%22:{%22native_token%22:{%22denom%22:%22uusd%22}},%22ask_asset_info%22:{%22native_token%22:{%22denom%22:%22uluna%22}}}}]}}

# Router - Terra 2.0


## Transaction

### Execute swap operations

Multi-hop swap operations via `terra_swap`.<br />

Case 1) The first source token is a native token (Luna => DELIGHT => TNT)<br />

```json
{
   "execute_swap_operations":{
      "operations":[
         {
            "terra_swap":{
               "offer_asset_info":{
                  "native_token":{
                     "denom":"uluna"
                  }
               },
               "ask_asset_info":{
                  "token":{
                     "contract_addr":"terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24"
                  }
               }
            }
         },
         {
            "terra_swap":{
               "offer_asset_info":{
                  "token":{
                     "contract_addr":"terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24"
                  }
               },
               "ask_asset_info":{
                  "token":{
                     "contract_addr":"terra1qnypzwqa03h8vqs0sxjp8hxw0xy5zfwyax26jgnl5k4lw92tjw0scdkrzm"
                  }
               }
            }
         }
      ],
      "minimum_receive":"1"
   }
}
```

Case 2) The first source token is a CW20 token (DELIGHT => Luna => TNT)<br />

Note: `Binary()` means that this JSON message should be encoded into Base64.<br />

```json
{
  "send": {
    "amount": "100000000",
    "contract": "terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24",
    "msg": Binary({
        "execute_swap_operations":{
            "operations":[
                {
                    "terra_swap":{
                        "offer_asset_info":{
                            "token":{
                                "contract_addr":"terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24"
                            }
                        },
                        "ask_asset_info":{
                            "native_token":{
                                "denom":"uluna"
                            }
                        }
                    }
                },
                {
                    "terra_swap":{
                        "offer_asset_info":{
                            "native_token":{
                                "denom":"uluna"
                            }
                        },
                        "ask_asset_info":{
                            "token":{
                                "contract_addr":"terra1qnypzwqa03h8vqs0sxjp8hxw0xy5zfwyax26jgnl5k4lw92tjw0scdkrzm"
                            }
                        }
                    }
                }
            ],
            "minimum_receive":"1"
        }
    })
}
```

## Query

### Simulate swap operations

Number of TNT that can be get through 10 Luna <br>
```json
{
    "simulate_swap_operations":{
        "offer_amount":"10000000",
        "operations":[
            {
                "terra_swap":{
                    "offer_asset_info":{
                        "native_token":{
                            "denom":"uluna"
                        }
                    },
                    "ask_asset_info":{
                        "token":{
                            "contract_addr":"terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24"
                        }
                    }
                }
            },
            {
                "terra_swap":{
                    "offer_asset_info":{
                        "token":{
                            "contract_addr":"terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24"
                        }
                    },
                    "ask_asset_info":{
                        "token":{
                            "contract_addr":"terra1qnypzwqa03h8vqs0sxjp8hxw0xy5zfwyax26jgnl5k4lw92tjw0scdkrzm"
                        }
                    }
                }
            }
        ]
    }
}
```
ex) https://bombay-fcd.terra.dev/wasm/contracts/terra1c58wrdkyc0ynvvxcv834kz65nfsxmw2w0pwusq/store?query_msg={%22simulate_swap_operations%22:{%22offer_amount%22:%2210000000%22,%22operations%22:[{%22terra_swap%22:{%22offer_asset_info%22:{%22token%22:{%22contract_addr%22:%22terra1747mad58h0w4y589y3sk84r5efqdev9q4r02pc%22}},%22ask_asset_info%22:{%22native_token%22:{%22denom%22:%22uusd%22}}}},{%22native_swap%22:{%22offer_denom%22:%22uusd%22,%22ask_denom%22:%22ukrw%22}}]}}

### Reverse simulate swap operations
Number of Luna needed to get 10 TNT <br>
```json
{
    "reverse_simulate_swap_operations":{
        "ask_amount":"10000000",
        "operations":[
            {
                "terra_swap":{
                    "offer_asset_info":{
                        "native_token":{
                            "denom":"uluna"
                        }
                    },
                    "ask_asset_info":{
                        "token":{
                            "contract_addr":"terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24"
                        }
                    }
                }
            },
            {
                "terra_swap":{
                    "offer_asset_info":{
                        "token":{
                            "contract_addr":"terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24"
                        }
                    },
                    "ask_asset_info":{
                        "token":{
                            "contract_addr":"terra1qnypzwqa03h8vqs0sxjp8hxw0xy5zfwyax26jgnl5k4lw92tjw0scdkrzm"
                        }
                    }
                }
            }
        ]
    }
}
```
