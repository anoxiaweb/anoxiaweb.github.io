<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>金融科普咨询平台</title>
    <style>
        * {
            box-sizing: border-box;
            margin:0;
            padding:0;
        }
        body {
            background-color: #f5f7fa;
            font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            color:#1f2937;
        }
        /* 弹窗遮罩 */
        .modal-mask{
            position:fixed;
            inset:0;
            background:rgba(0,0,0,0.6);
            z-index:999;
            display:flex;
            align-items:center;
            justify-content:center;
            padding:1rem;
        }
        .modal-box{
            background:#fff;
            border-radius:0.75rem;
            max-width:32rem;
            width:100%;
            padding:1.5rem;
        }
        .modal-scroll{
            max-height:18rem;
            overflow:auto;
            font-size:0.875rem;
            color:#4b5563;
            margin-bottom:1rem;
        }
        .btn-group{
            display:flex;
            gap:0.75rem;
        }
        .btn{
            flex:1;
            padding:0.5rem 1rem;
            border-radius:0.5rem;
            font-size:1rem;
        }
        .btn-outline{
            border:1px solid #d1d5db;
            color:#4b5563;
            background:white;
        }
        .btn-primary{
            background:#2563eb;
            color:white;
            border:none;
        }
        /* header */
        header{
            background:#1d4ed8;
            color:white;
            box-shadow:0 4px 6px rgba(0,0,0,0.1);
        }
        .header-inner{
            max-width:64rem;
            margin:0 auto;
            padding:0.75rem 1rem;
            display:flex;
            justify-content:space-between;
            align-items:center;
        }
        .header-title{
            font-size:1.25rem;
            font-weight:bold;
        }
        .header-buttons button{
            background:transparent;
            border:none;
            color:white;
            font-size:0.875rem;
            margin-left:1rem;
            cursor:pointer;
        }
        .header-buttons button.active{
            text-decoration:underline;
            font-weight:bold;
        }
        /* 警告提示条 */
        .warn-bar{
            background:#fffbeb;
            border-bottom:1px solid #fcd34d;
            color:#92400e;
            font-size:0.875rem;
            text-align:center;
            padding:0.5rem 1rem;
        }
        main{
            max-width:64rem;
            margin:0 auto;
            padding:1.5rem 1rem;
        }
        h2{
            font-size:1.625rem;
            font-weight:bold;
            margin-bottom:0.5rem;
        }
        .desc-text{
            color:#4b5563;
            margin-bottom:2rem;
        }
        .card-grid{
            display:grid;
            grid-template-columns: repeat(auto-fit, minmax(240px,1fr));
            gap:1.25rem;
            margin:2.5rem 0;
        }
        .card-item{
            background:#fff;
            border:1px solid #e5e7eb;
            border-radius:0.75rem;
            padding:1.5rem;
            box-shadow:0 1px 3px rgba(0,0,0,0.05);
            cursor:pointer;
            transition:0.25s;
        }
        .card-item:hover{
            transform:translateY(-3px);
            box-shadow:0 6px 14px rgba(0,0,0,0.1);
        }
        .card-item h3{
            font-size:1.125rem;
            font-weight:bold;
            margin-bottom:0.5rem;
        }
        .card-item p{
            font-size:0.875rem;
            color:#6b7280;
        }
        .full-btn{
            width:100%;
            background:#1d4ed8;
            color:white;
            border:none;
            padding:0.75rem;
            border-radius:0.5rem;
            font-size:1.125rem;
            cursor:pointer;
        }
        .info-panel{
            margin-top:2.5rem;
            padding:1.25rem;
            background:#eff6ff;
            border:1px solid #bfdbfe;
            border-radius:0.75rem;
            text-align:center;
        }
        .wx-text{
            background:#fff;
            display:inline-block;
            padding:0.5rem 1rem;
            border:1px solid #d1d5db;
            border-radius:0.5rem;
            font-family:monospace;
            color:#1d4ed8;
            font-weight:bold;
            margin:0.5rem 0;
        }
        .small-note{
            font-size:0.75rem;
            color:#9ca3af;
        }
        /* 聊天页面 */
        .chat-box{
            background:#fff;
            border:1px solid #d1d5db;
            border-radius:0.5rem;
            padding:1rem;
            height:380px;
            overflow-y:auto;
            margin-bottom:1rem;
        }
        .chat-empty{
            color:#9ca3af;
            text-align:center;
            padding:3rem 0;
        }
        .msg-user{
            margin:0.75rem 0;
            text-align:right;
        }
        .msg-user span{
            background:#dbeafe;
            padding:0.5rem 0.75rem;
            border-radius:0.5rem;
            display:inline-block;
            max-width:85%;
        }
        .msg-bot{
            margin:0.75rem 0;
        }
        .msg-bot span{
            background:#f3f4f6;
            padding:0.5rem 0.75rem;
            border-radius:0.5rem;
            display:inline-block;
            max-width:85%;
            white-space:pre-wrap;
        }
        .chat-input-row{
            display:flex;
            gap:0.5rem;
        }
        #chatInput{
            flex:1;
            border:1px solid #d1d5db;
            border-radius:0.5rem;
            padding:0.5rem 0.75rem;
            font-size:1rem;
        }
        .send-btn{
            background:#1d4ed8;
            color:white;
            border:none;
            padding:0.5rem 1.25rem;
            border-radius:0.5rem;
            cursor:pointer;
        }
        footer{
            margin-top:3rem;
            padding:1.25rem 0;
            border-top:1px solid #e5e7eb;
            text-align:center;
            font-size:0.875rem;
            color:#9ca3af;
        }
        .toast{
            position:fixed;
            top:1rem;
            left:50%;
            transform:translateX(-50%);
            background:rgba(0,0,0,0.75);
            color:white;
            padding:0.5rem 1.25rem;
            border-radius:0.5rem;
            z-index:998;
        }
    </style>
</head>
<body>
<div id="app"></div>

<script>
const STORAGE_KEYS = {
    chatList: "fin_chat",
    sensitiveWords: "fin_sensitive",
    knowledgeBase: "fin_knowledge"
}

// =========在这里修改你的微信号！！=========
const MY_WECHAT = "za0613qqqq";
// ==============================================

const defaultSensitive = [
    "帮我贷款","黑户下款","网贷口子","找放款渠道","代办贷款","帮我申请贷款",
    "无视征信借钱","洗白征信","消除逾期","强制下款","付费修复征信","中介代办借款"
]

const defaultKnowledge = [
    {
        q:"征信逾期有什么影响",
        keywords:["征信逾期","逾期后果","逾期影响"],
        a:"征信发生逾期之后，这条逾期记录会保存在个人征信报告中。申请房贷、车贷、信用卡以及各类银行信贷产品时，银行会查看你的征信，逾期记录会降低审批通过率，严重逾期会直接拒绝。逾期欠款全部结清后，逾期记录会保留5年，5年后自动删除；欠款如果一直没有还清，逾期记录会永久保留在报告上。网上宣称花钱消除征信逾期全部是诈骗，征信记录无法人为篡改。"
    },
    {
        q:"查询征信次数太多会怎样",
        keywords:["查询次数","硬查询","征信查多了","查询过多"],
        a:"征信查询分为本人查询和机构硬查询。自己在官网查询属于本人查询，不会对征信造成负面影响；当你申请信用卡、网贷、贷款，金融机构查询你的征信，这叫硬查询。短时间内硬查询次数过多，银行会判断你资金紧张、到处借钱，会降低贷款审批通过率。建议半年内不要频繁申请信贷产品，一年自查征信2次就足够。"
    },
    {
        q:"什么是负债率",
        keywords:["负债率","负债比例","收入负债比"],
        a:"负债率是衡量个人负债压力的指标，一般分为资产负债率和收入负债率。信贷审批参考较多的是收入负债率，即每月还款总额占月收入的比重。多数银行希望申请人负债率不要超过50%，负债太高，还款压力大，银行会认为还款风险高，贷款容易被拒绝。想要降低负债率，可以结清部分网贷、消费贷，减少每月还款额。"
    },
    {
        q:"网贷多了有什么后果",
        keywords:["网贷多","网贷太多","网贷笔数多"],
        a:"网贷笔数多，就算全部按时还款没有逾期，也会带来不少负面影响。第一，拉高个人整体负债水平；第二，产生大量征信查询记录；第三，很多银行会介意客户网贷过多，申请房贷、信用卡时更容易被拒。建议尽量少碰网贷，不要以贷养贷，如果有多余资金优先结清高利息网贷。"
    },
    {
        q:"如何维护个人征信",
        keywords:["维护征信","养好征信","养征信"],
        a:"维护征信记住几点：1、所有信用卡、贷款按时足额还款，坚决不要逾期；2、不要随便点网贷额度测试，每点一次很多时候就会留下一次征信查询；3、控制信贷申请频率，不要短时间多头申请；4、每年1‑2次登录央行征信中心官网自查征信；5、不要随意帮别人做担保，对方逾期会直接记在你的征信报告上。"
    },
    {
        q:"逾期之后多久消除记录",
        keywords:["逾期多久消除","逾期记录多久没","逾期清除时间"],
        a:"征信逾期记录不是逾期结束立刻消失。规则是：欠款全部结清当天开始计算，保留5年，5年后系统自动消除。如果你一直不还钱，逾期记录就会一直存在，不会自动消失。所以出现逾期第一件事就是尽快把欠款结清。"
    },
    {
        q:"负债过高怎么办",
        keywords:["负债过高","负债太高","负债压力大"],
        a:"负债过高不要慌张，首先停止继续申请新的贷款网贷，避免债务继续增加。梳理清楚自己所有负债，分清哪些利息高、哪些利息低。优先偿还利息高的网贷，减少利息压力。做好收支记账，缩减非必要消费。千万不要以贷养贷，借新还旧只会让债务越滚越大。如果压力过大，可以和正规金融机构沟通协商还款方案。"
    },
    {
        q:"什么叫连三累六",
        keywords:["连三累六"],
        a:"连三，就是连续三个月没有按时还款，连续逾期；累六，两年之内累计发生六次逾期记录。连三累六属于征信严重不良，绝大多数银行会直接拒绝你的贷款、信用卡申请。出现这种情况，需要结清欠款，等待记录随时间消除。"
    },
    {
        q:"怎么查询个人征信报告",
        keywords:["查征信","征信报告在哪里查","打印征信"],
        a:"可以登录中国人民银行征信中心官网，这是官方渠道，每年有2次免费简版征信查询机会；也可以携带本人身份证，前往人民银行网点或者授权商业银行网点打印详版征信报告。不要找第三方中介代查征信，会泄露你的个人身份信息。"
    },
    {
        q:"中介说可以修复征信可信吗",
        keywords:["修复征信","洗白征信","中介修复征信"],
        a:"完全不可信，属于典型金融骗局。征信记录是各个信贷机构如实上报给征信中心，不存在花钱洗白、付费修复逾期的操作。中介拿到你的身份材料之后，要么骗你的服务费，甚至拿你的信息做别的事情。只有欠款结清等待5年才会自动消除逾期。"
    },
    {
        q:"征信报告上面的查询记录代表什么",
        keywords:["查询记录","征信报告查询","贷后管理"],
        a:"征信查询记录会记录每一次查询主体和查询时间。本人查询是你自己查看征信；贷款审批、信用卡审批属于机构硬查询；贷后管理是银行对你已有账户进行定期复查，贷后管理查询一般不会影响审批。看征信重点看硬查询数量。"
    },
    {
        q:"什么叫以贷养贷",
        keywords:["以贷养贷"],
        a:"以贷养贷就是不断申请新的网贷、贷款，拿新借到的钱去还旧的欠款。这种做法短期看上去解决还款压力，但是每一笔借款都会产生利息，债务会越滚越大，最后很容易全面逾期。一定要尽量避免这种行为。"
    },
    {
        q:"网贷不还会怎么样",
        keywords:["网贷不还","网贷逾期不还"],
        a:"正规持牌机构的网贷逾期不还，会上报征信留下逾期记录，产生罚息违约金，平台会进行催收，长时间拖欠，还可能被机构起诉，影响个人正常生活。不要抱有网贷可以不还的想法。"
    },
    {
        q:"担保对征信有什么影响",
        keywords:["担保","做担保","贷款担保"],
        a:"给别人做贷款担保，这笔负债会同时显示在你的征信报告上。如果对方没有按时还款发生逾期，逾期记录直接记到你的征信，同时你有义务代为偿还这笔债务。不要轻易给他人做担保。"
    },
    {
        q:"什么是呆账",
        keywords:["呆账","征信呆账"],
        a:"呆账属于征信里很严重的状态，代表欠款长期无法收回。出现呆账，几乎所有银行信贷业务都会直接拒批。分为一般呆账、溢缴款呆账。普通欠款呆账，必须把欠款本息全部结清，结清后依旧保留5年记录；溢缴款呆账是信用卡多存钱未取出，联系银行处理即可消除。"
    },
    {
        q:"什么是止付",
        keywords:["止付","信用卡止付"],
        a:"信用卡止付，卡片被银行限制使用，分为用户主动止付和银行被动止付。主动止付是自己挂失、申请冻结卡片；银行止付常见原因有逾期、套现风险、异常交易。止付状态会写进征信，需要联系银行解除。"
    },
    {
        q:"什么是销户",
        keywords:["销户","信用卡销户"],
        a:"销户就是彻底注销信用卡账户。销户不等于记录立刻消失，销户之后，该账户下的还款记录依旧保留5年。如果有逾期，必须结清欠款再销户，逾期记录才会按5年周期计时。逾期没结清就销户，不良记录会一直留在征信。"
    },
    {
        q:"简版征信和详版征信区别",
        keywords:["简版征信","详版征信","简版详版区别"],
        a:"简版征信：网上征信中心官网查询，信息精简，适合日常自查。详版征信：线下网点打印，信息完整，包含每一笔账户明细、还款记录、完整查询记录，办理房贷车贷银行必须看详版征信。"
    },
    {
        q:"信用卡最低还款对征信有影响吗",
        keywords:["最低还款","信用卡最低还款"],
        a:"选择信用卡最低还款，不会造成征信逾期，征信上不会记不良。但是银行会计收循环利息，利息成本很高，长期最低还款，部分银行会判定客户资金紧张，后续信贷审批会受一定影响。"
    },
    {
        q:"信用卡分期影响征信吗",
        keywords:["信用卡分期","账单分期"],
        a:"正常办理信用卡分期，按时还款，不会产生不良记录。但是分期的负债会计入你的总负债，银行审批贷款会把分期每月还款计入负债，拉高你的负债率，负债过高会影响贷款审批。"
    },
    {
        q:"征信上显示已结清是什么意思",
        keywords:["已结清","账户已结清"],
        a:"账户状态显示已结清，代表这笔贷款或者信用卡欠款已经全部还清。结清之后，账户历史还款记录依旧会保留在征信5年，良好的还款记录对个人征信是有利的。"
    },
    {
        q:"没使用的网贷需要注销吗",
        keywords:["没用的网贷","注销网贷账户","网贷账户"],
        a:"就算网贷没有借款，只要授信成功，征信上就会显示该授信账户。建议不用的网贷做账户注销。保留过多授信账户，银行审批时会认为你潜在负债风险高，容易影响房贷信用卡审批。"
    },
    {
        q:"查询征信会留下记录吗",
        keywords:["查征信留记录","本人查询记录"],
        a:"本人自己查询征信，会留下本人查询记录，这种记录不会影响信贷审批。只有贷款审批、信用卡审批这类机构硬查询，才会对审批产生影响，不用害怕自己查征信。"
    },
    {
        q:"逾期一天会上征信吗",
        keywords:["逾期一天","晚还一天上征信吗"],
        a:"部分银行信用卡提供1‑3天还款宽限期，宽限期内还款不会上报征信。大部分网贷、消费贷没有宽限期，还款日没按时还款直接上报逾期。不要依靠宽限期，尽量还款日当天完成还款。"
    },
    {
        q:"什么叫多头借贷",
        keywords:["多头借贷","多头授信"],
        a:"多头借贷就是同时在多家机构办理贷款、网贷。征信上面会出现大量不同机构授信账户和硬查询记录。银行风控非常反感多头借贷，代表资金缺口大，信贷审批很容易被拒。"
    },
    {
        q:"信用卡销户和销卡区别",
        keywords:["销卡","销户","销卡销户区别"],
        a:"销卡：只注销单张信用卡卡片，账户依旧保留；销户：注销整个信用卡账户，名下全部卡片一并注销。有逾期欠款，必须结清，再办理销卡或者销户，不良记录才会按5年消除。"
    },
    {
        q:"夫妻之间征信会互相影响吗",
        keywords:["夫妻征信","夫妻互相影响征信"],
        a:"个人征信报告独立，单方面逾期不会直接写进另一方征信。但是办理房贷这类共同负债业务，夫妻双方征信都会审核，一方征信很差，房贷会直接被拒。共同贷款，双方都承担还款责任，任意一方逾期，两个人征信都会记逾期。"
    },
    {
        q:"征信报告空白好不好",
        keywords:["白户","征信白户","空白征信"],
        a:"征信白户就是没有任何信贷记录。没有逾期记录，但是银行看不到你的还款履约记录，部分银行审批会谨慎对待。可以适度使用信用卡正常履约，积累良好征信记录。"
    },
    {
        q:"什么是黑户",
        keywords:["征信黑户","黑户"],
        a:"黑户不是官方名词，民间指征信存在严重不良，比如连三累六、呆账，多家机构逾期。黑户很难申请信用卡、银行贷款。需要结清全部欠款，等待5年不良记录自动清除。不存在黑户可以无视征信下款的正规产品。"
    },
    {
        q:"什么是LPR",
        keywords:["LPR","贷款市场报价利率"],
        a:"LPR即贷款市场报价利率，由全国多家银行报价，每月20号更新，是国内房贷、经营性贷款的重要参考基准，分为1年期、5年期以上两个品种。房贷主要参考5年期以上LPR。LPR变动，你的浮动利率贷款利息会跟着变化；固定利率不受LPR调整影响。注意：LPR是参考基准，不是你的实际执行利率，银行会在LPR基础上加点形成最终利率。"
    },
    {
        q:"基点BP是什么意思",
        keywords:["基点","BP","加点","减点"],
        a:"BP就是基点，1BP=0.01%。例如LPR是3.95%，银行加30BP，实际执行利率=3.95%+0.30%=4.25%。加点数值写进贷款合同，终身固定，不会随LPR变化。只有LPR数值每月变动，你的利率才会变动。"
    },
    {
        q:"等额本息和等额本金区别",
        keywords:["等额本息","等额本金","两种还款方式"],
        a:"①等额本息：每个月月供金额完全一样。前期还的利息占比高，本金占比低；总利息更高，适合收入稳定普通家庭。②等额本金：每月归还固定本金，利息逐月变少，月供逐月递减。前期月供压力很大，越还越轻松，整体总利息更少，适合收入偏高、可以承受前期高月供的人群。两种方式都是按剩余本金计算利息，不存在利息多付一倍的情况。"
    },
    {
        q:"什么是先息后本",
        keywords:["先息后本","先息后本还款"],
        a:"先息后本：每个月只支付利息，到期一次性归还全部本金。常见于经营抵押贷。优点：月供压力小，资金利用率高；风险点：到期一次性还本压力巨大，如果经营回款不及预期，到期容易资金断裂。银行会审核你的经营流水，不是所有人都可以申请先息后本。"
    },
    {
        q:"什么是垫资过桥",
        keywords:["垫资","过桥","过桥资金","赎楼垫资"],
        a:"垫资过桥（赎楼）是短期民间周转资金。场景：房子原有按揭/抵押未结清，需要借钱把旧贷款结清、解押，再办理新抵押，新抵押放款之后归还这笔短期垫资。特点：周期很短，一般几天到几个月；成本按天计息，综合成本很高，属于高风险资金。只适合短期周转，绝对不可以长期使用。"
    }
]

// 强制清空旧脏缓存，解决本地file://缓存bug
localStorage.removeItem(STORAGE_KEYS.chatList);
localStorage.removeItem(STORAGE_KEYS.sensitiveWords);
localStorage.removeItem(STORAGE_KEYS.knowledgeBase);

// 初始化存储
localStorage.setItem(STORAGE_KEYS.sensitiveWords, JSON.stringify(defaultSensitive))
localStorage.setItem(STORAGE_KEYS.knowledgeBase, JSON.stringify(defaultKnowledge))
localStorage.setItem(STORAGE_KEYS.chatList, JSON.stringify([]))

function getStorage(key){
    return JSON.parse(localStorage.getItem(key))
}
function setStorage(key,val){
    localStorage.setItem(key,JSON.stringify(val))
}

let page = "home"
let showAgreeModal = true
let toastMsg = ""

function checkSensitive(text){
    const words = getStorage(STORAGE_KEYS.sensitiveWords)
    for(let w of words){
        if(text.includes(w)) return true
    }
    return false
}

function getLocalAnswer(question){
    const kb = getStorage(STORAGE_KEYS.knowledgeBase)
    const qClean = question.trim().toLowerCase();
    for(let item of kb){
        for(let kw of item.keywords){
            if(qClean.includes(kw.toLowerCase())){
                return item.a
            }
        }
        if(qClean.includes(item.q.toLowerCase()) || item.q.toLowerCase().includes(qClean)){
            return item.a
        }
    }
    return "感谢咨询，本平台仅做征信信贷科普演示。你可以提问征信逾期、LPR利率、房贷、抵押贷、垫资过桥、还款方式、担保、金融防骗等常识问题。本平台不办理任何借贷业务，办理信贷请前往持牌银行机构。"
}

function sendMsg(inputText){
    const text = inputText.trim()
    if(!text){ toastMsg = "请输入咨询问题"; setTimeout(()=>{toastMsg="";render()},1200); render(); return; }
    if(checkSensitive(text)){ toastMsg = "该内容不在服务范围，禁止贷款相关咨询"; setTimeout(()=>{toastMsg="";render()},1500); render(); return; }

    const chatList = getStorage(STORAGE_KEYS.chatList)
    chatList.push({role:"user",content:text})
    const answer = getLocalAnswer(text);
    chatList.push({role:"bot",content:answer})
    setStorage(STORAGE_KEYS.chatList,chatList)
    render();
}

function render(){
    let html = ""
    if(showAgreeModal){
        html += `
        <div class="modal-mask">
            <div class="modal-box">
                <h3 style="font-size:1.25rem;font-weight:bold;margin-bottom:0.75rem;">用户协议、隐私政策与免责声明</h3>
                <div class="modal-scroll">
                    <p><strong>免责声明：</strong>本网站仅作为教学演示项目，只提供金融知识科普，不提供贷款、征信修复、代办融资、垫资过桥等真实业务。网站所有内容仅供学习参考，不构成任何投资、借贷建议。依据本站内容做出的行为风险由使用者自行承担。</p>
                    <br>
                    <p><strong>用户协议：</strong>1.本系统全部数据保存在浏览器本地，清除缓存数据将全部丢失；2.禁止发布借贷引流、诈骗相关内容；3.禁止利用本演示系统开展盈利经营活动。</p>
                    <br>
                    <p><strong>隐私政策：</strong>本演示不会收集、上传你的个人隐私信息，对话记录全部存储在你的本机浏览器。</p>
                </div>
                <div class="btn-group">
                    <button class="btn btn-outline" id="btnDisagree">拒绝并退出</button>
                    <button class="btn btn-primary" id="btnAgree">我已阅读并同意</button>
                </div>
            </div>
        </div>
        `
    }
    if(toastMsg){
        html += `<div class="toast">${toastMsg}</div>`
    }
    html += `
    <header>
        <div class="header-inner">
            <div class="header-title">金融科普咨询平台</div>
            <div class="header-buttons">
                <button data-page="home" class="${page==='home'?'active':''}">首页</button>
                <button data-page="chat" class="${page==='chat'?'active':''}">在线问答</button>
            </div>
        </div>
    </header>
    <div class="warn-bar">
        ⚠️温馨提示：本平台仅科普，不办理贷款。凡是声称洗白征信、强制下款均属于骗局，请保护个人财产。
    </div>
    <main>
    `
    if(page === "home"){
        html += `
        <section>
            <h2>金融科普咨询服务</h2>
            <p class="desc-text">解答征信、利率LPR、房贷、房产抵押、垫资过桥、还款方式、担保、金融防骗等知识，仅做科普演示，不办理真实贷款业务。</p>
            <div class="card-grid">
                <div data-goto="chat" class="card-item">
                    <h3>征信解读</h3>
                    <p>逾期后果、征信名词、征信报告解读相关科普</p>
                </div>
                <div data-goto="chat" class="card-item">
                    <h3>利率与还款</h3>
                    <p>LPR、基点、等额本息、先息后本、真实年化讲解</p>
                </div>
                <div data-goto="chat" class="card-item">
                    <h3>房贷抵押</h3>
                    <p>按揭、经营抵押、消费抵押、解押二押知识科普</p>
                </div>
                <div data-goto="chat" class="card-item">
                    <h3>过桥与风险</h3>
                    <p>垫资赎楼、担保责任、贷款合同、金融防骗科普</p>
                </div>
            </div>
            <button data-goto="chat" class="full-btn">进入在线问答</button>

            <div class="info-panel">
                <h4 style="font-weight:bold;font-size:1.125rem;margin-bottom:0.5rem;">需要人工进一步咨询？</h4>
                <p style="color:#4b5563;">复制微信号，打开微信添加好友咨询</p>
                <div class="wx-text">${MY_WECHAT}</div>
                <p class="small-note">本渠道仅做知识交流，不办理借贷业务</p>
            </div>
        </section>
        `
    }else if(page === "chat"){
        const chatList = getStorage(STORAGE_KEYS.chatList)
        html += `<h2>在线咨询问答</h2>`
        html += `<p style="font-size:0.875rem;color:#6b7280;margin-bottom:0.75rem;">📄内置知识库，直接输入名词提问，例如：LPR、先息后本、垫资过桥、二押、基点BP</p>`
        html += `<div class="chat-box" id="chatBox">`
        if(chatList.length === 0){
            html += `<div class="chat-empty">示例提问：什么是LPR？垫资过桥风险？等额本息和等额本金区别？</div>`
        }
        for(let item of chatList){
            if(item.role === "user"){
                html += `<div class="msg-user"><span>${item.content}</span></div>`
            }else{
                html += `<div class="msg-bot"><span>${item.content}</span></div>`
            }
        }
        html += `</div>`
        html += `
        <div class="chat-input-row">
            <input id="chatInput" placeholder="输入金融名词或者问题">
            <button id="sendBtn" class="send-btn">提交咨询</button>
        </div>
        <p class="small-note" style="margin-top:0.5rem;">禁止提问贷款、洗白征信类问题，系统将自动拦截敏感提问。</p>

        <div class="info-panel">
            <p style="color:#4b5563;">如需人工深度科普交流，复制微信号：<span class="wx-text">${MY_WECHAT}</span>，前往微信添加</p>
            <p class="small-note">不提供借贷办理服务</p>
        </div>
        `
    }
    html += `
    </main>
    <footer>©2026 金融科普咨询平台｜教学演示项目，不提供真实金融业务服务</footer>
    `
    document.getElementById("app").innerHTML = html

    //聊天框滚动到底部
    const boxDom = document.getElementById("chatBox")
    if(boxDom) boxDom.scrollTop = boxDom.scrollHeight
}

render();

// =====全局事件委托！！全部事件放外层，不再render里面绑定（核心修复）=====
document.body.addEventListener("click",function(e){
    const target = e.target;
    //协议弹窗
    if(target.id === "btnAgree"){
        showAgreeModal = false;
        render();
        return;
    }
    if(target.id === "btnDisagree"){
        alert("必须阅读并同意协议才可以访问网站");
        return;
    }
    //页面切换按钮 data-page
    if(target.hasAttribute('data-page')){
        page = target.dataset.page;
        render();
        return;
    }
    //跳转聊天 data-goto
    if(target.hasAttribute('data-goto')){
        page = target.dataset.goto;
        render();
        return;
    }
    //提交咨询按钮
    if(target.id === "sendBtn"){
        const inputDom = document.getElementById("chatInput");
        sendMsg(inputDom.value);
        inputDom.value = "";
    }
})

//回车发送，全局监听keydown
document.body.addEventListener("keydown",function(e){
    if(e.key === 'Enter' && document.getElementById("chatInput")){
        const inputDom = document.getElementById("chatInput");
        sendMsg(inputDom.value);
        inputDom.value = "";
    }
})
</script>
</body>
</html>
