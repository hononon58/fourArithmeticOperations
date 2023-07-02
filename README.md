# fourArithmeticOperations


namespace ArithmeticOperations
{
    public class Calculator
    {
        private string _input;
        private int _result;

        public Calculator(string input)
        {
            _input = input;
            _result = Controller();
        }
        private int Controller()
        {
            var inputList = Spilit();
            var stringList = new List<string>();
            foreach (var input in inputList)
            {
                var numberList = Separate(input);
                if (!numberList.Any())
                {
                    continue;
                }
                if (!int.TryParse(numberList[0], out int number) && numberList.Count > 1)
                {
                    stringList.Add(numberList[0]);
                }
                foreach (var num in Calculate(numberList))
                {
                    stringList.Add(num);
                }
            }
            int.TryParse(Calculate(stringList)[0], out int result);
            return _result = result;
        }
        private IEnumerable<string> Spilit()
        {
            return _input.Split('(', ')').ToList();
        }
        private List<string> Separate(string input)
        {
            var stringList = input.Select(str => str.ToString()).ToList();
            var list = new List<string>();
            string number = string.Empty;
            int counter = 0;
            foreach (var num in stringList)
            {
                if (int.TryParse(num, out int n))
                {
                    number += n;
                    if (counter == (stringList.Count - 1))
                    {
                        list.Add(number);
                    }
                }
                else
                {
                    if (number != string.Empty)
                    {
                        list.Add(number);
                        number = string.Empty;
                    }
                    list.Add(num);
                }
                counter++;
            }
            return list;
        }
        private List<string> Calculate(List<string> inputList)
        {
            int.TryParse(inputList[0], out int first);
            int result = first;
            foreach (string element in inputList)
            {
                if (element == "+")
                {
                    if (inputList.Count <= inputList.IndexOf(element) + 1)
                    {
                        return new List<string> { result.ToString(), element };
                    }
                    var number = inputList[inputList.IndexOf(element) + 1];
                    int.TryParse(number, out int num);
                    result += num;
                }
                if (element == "-")
                {
                    if (inputList.Count <= inputList.IndexOf(element) + 1)
                    {
                        return new List<string> { result.ToString(), element };
                    }
                    var number = inputList[inputList.IndexOf(element) + 1];
                    int.TryParse(number, out int num);
                    result -= num;
                }
                if (element == "*")
                {
                    if (inputList.Count <= inputList.IndexOf(element) + 1)
                    {
                        return new List<string> { result.ToString(), element };
                    }
                    var number = inputList[inputList.IndexOf(element) + 1];
                    int.TryParse(number, out int num);
                    result = result * num;
                }
                if (element == "/")
                {
                    if (inputList.Count <= inputList.IndexOf(element) + 1)
                    {
                        if (result == 0)
                        {
                            return new List<string> { element };
                        }
                        return new List<string> { result.ToString(), element };
                    }
                    var number = inputList[inputList.IndexOf(element) + 1];
                    int.TryParse(number, out int num);
                    result = result / num;
                }

            }
            return new List<string> { result.ToString() };
        }
        public override string ToString()
        {
            return $"{_input}={_result}";
        }
    }
}
