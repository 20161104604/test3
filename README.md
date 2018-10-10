# test3
test
import UIKit

class ViewController: UIViewController {
    @IBOutlet weak var xianshi: UILabel!
    @IBOutlet weak var one: UIButton!
    @IBOutlet weak var two: UIButton!
    @IBOutlet weak var three: UIButton!
    @IBOutlet weak var four: UIButton!
    @IBOutlet weak var five: UIButton!
    @IBOutlet weak var six: UIButton!
    @IBOutlet weak var seven: UIButton!
    @IBOutlet weak var eight: UIButton!
    @IBOutlet weak var nine: UIButton!
    @IBOutlet weak var zero: UIButton!
    @IBOutlet weak var show: UILabel!
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
}
import UIKit
class ViewController: UIViewController
{
    @IBOutlet weak var display: UILabel!
    
    var userIsInTheMiddleOfTypingANumber:Bool = false
    //用户是否已经输入数字，由于Swift的变量必须负初始值，所以设为false
    
    @IBAction func appendDigit(sender: UIButton){
        let digit = sender.currentTitle!//直接获取Button的数字
        
        //若已输入过数字，则直接往display中添加数字，否则直接现实新点击数字，去除原始0的操作
        if userIsInTheMiddleOfTypingANumber{
            display.text = display.text! + digit
        }else{
            display.text = digit
            userIsInTheMiddleOfTypingANumber = true
        }
    }
    
    //对数字进行运算
    @IBAction func operate(sender: UIButton) {
        let operation = sender.currentTitle!
        if userIsInTheMiddleOfTypingANumber{
            enter()
        }
        switch operation{
            /*swift算法极为简洁，当调用方法performOperation时，其自动对比方法的参数，而无需在
             *调用方法时写明参数类型，例如，这里的参数$0 与 $1并没有指明类型，而Swift会直接将其适应为
             *方法performOpetation中的Double型
             */
        case "×": performOperation { $0 * $1 }
        case "÷": performOperation { $1 / $0 }
        case "+": performOperation { $0 + $1 }
        case "−": performOperation { $1 - $0 }
        case "√": performOperation { sqrt($0) }
        default: break
            
        }
    }
    
    //两个参数进行运算的方法
    func performOperation(operation: (Double,Double) -> Double){
        if operandStack.count >= 2 {
            displayValue = operation(operandStack.removeLast(),operandStack.removeLast())
            enter()
        }
        
    }
    
    //一个参数进行运算的方法，Swift支持方法的重载，但Obj-C不允许，这里继承了Obj-C的
    //类UIViewColler，不能重载方法performOperation，故将其变为Private方法
    private func performOperation(operation: Double -> Double){
        if operandStack.count >= 1 {
            displayValue = operation(operandStack.removeLast())
            enter()
        }
        
    }
    var operandStack = Array<Double>()
    
    //若用户点击enter，则将相应数字添加至数组Array中
    @IBAction func enter() {
        userIsInTheMiddleOfTypingANumber = false
        operandStack.append(displayValue)
        println("operandStack = \(operandStack)")
    }
    var displayValue: Double {
        get{
            return NSNumberFormatter().numberFromString(display.text!)!.doubleValue
        }
        set{
            display.text = "\(newValue)"
            userIsInTheMiddleOfTypingANumber = false
        }
    }
}


