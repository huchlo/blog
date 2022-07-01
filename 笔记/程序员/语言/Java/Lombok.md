  
    //欠费关阀设置  
    @ResponseBody  
    @ApiOperation(value = "网点欠费关阀设置", httpMethod = "POST")  
    @PostMapping(value = "/editBranchValveConfig", produces = RsmsConstant.JSON_UTF_8)  
    public JsonResult editBranchValveConfig(@RequestBody @Validated QueryExportLog queryExportLog) throws Exception {  
        //brandId 数字 123        //valveConfigStatus 1.欠费不关阀; 2. 欠费关阀  
        //valveConfigValue  数字 -99,99        return ReturnUtil.success("网点支付设置成功");  
//        return new JsonResult<>();  
    }  
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
    static class PayConfig implements Serializable {  
  
        @NotNull(message = "asd not null")  
        private String asd;  
  
    @NotNull(message = "zxc not null")  
        @Min(value = 0,message = "zxc not min")  
        private Integer zxc;  
    }  
    @ResponseBody  
    @ApiOperation(value = "获取网点支付配置", httpMethod = "POST")  
    @PostMapping(value = "/getBranchPayConfig", produces = RsmsConstant.JSON_UTF_8)  
    public JsonResult getBranchPayConfig( @RequestBody @Validated BranchController.PayConfig payConfig) throws Exception {  
        //只有微信小程序 商户号 设置  
        //先去t_branch_payment_config找  
        //无则去t_payment_channel找  
        System.out.println("asd");  
//        int i;  
PayConfig payConfig1 = new PayConfig();  
  
        //验证是否可以操作该公司  
        //checkIsOrNotOperateCompany(params.getCompanyId());  
        //根据公司ID查询其支付配置  
        //List<ResultCompanyPaymentConfig> companyPaymentConfigList = enterpriseInfoMapper.getCompanyPayConfigList(params.getCompanyId());  
  
        //查询默认的线上支付配置方式(一定不会为空)  
        //QueryPaymentChannel queryPaymentConfig = new QueryPaymentChannel();        //List<PaymentChannelVo> defaultOnLinePayment = paymentChannelMapper.queryDefaultOnlinePaymentChannel(queryPaymentConfig);        JsonResult<JSONObject> result = ReturnUtil.success("获取网点支付配置成功");  
        JSONObject jo = new JSONObject();  
        jo.put("subMchId","");  
        jo.put("enableStatus",0);  
        result.setData(jo);  
        return result;  
    }  
  
    @ResponseBody  
    @ApiOperation(value = "网点支付设置", httpMethod = "POST")  
    @PostMapping(value = "/editBranchPayConfig", produces = RsmsConstant.JSON_UTF_8)  
    public JsonResult editBranchPayConfig() throws Exception {  
        return ReturnUtil.success("网点支付设置成功");  
        //getOnLinePayment  
//        brandId 数字 123        //paymentConfigList [{}]        //appId: "41"        //key: "w"//        subMchId  
        //selected: 1选中 0未选中  
//        paymentConfigId:  
//        306  
//        return new JsonResult<>();  
//        JsonResult<String> result = ReturnUtil.success();  
//        JsonResult<JSONObject> result2 = ReturnUtil.success("获取网点支付配置成功");  
  
    }