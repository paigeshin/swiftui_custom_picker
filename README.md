# swiftui_custom_picker

```swift
//
//  ContentView.swift
//  CustomPickerView
//
//  Created by paige shin on 2023/01/17.
//

import SwiftUI

struct ContentView: View {
    
    @State var date = Date()
    
    var body: some View {
        ZStack {
            
            Color
                .black
                .ignoresSafeArea()
            
            VStack {
                Text("\(date)")
                    .padding()
                    .foregroundColor(.white)
                    
                Spacer()
                
                HStack(spacing: 0) {
                    CustomPicker(date: self.$date)
                }
                
                Spacer()
                
            } //: VSTACK
            
            
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

struct CustomPicker: UIViewRepresentable {
    
    @Binding var date: Date

    func makeUIView(context: UIViewRepresentableContext<CustomPicker>) -> UIPickerView {
        let picker: UIPickerView = UIPickerView()
        picker.dataSource = context.coordinator
        picker.delegate = context.coordinator
        let hour: Int = Calendar.current.component(.hour, from: self.date)
        picker.selectRow(hour, inComponent: 0, animated: false)
        return picker
    }
    
    func updateUIView(_ uiView: CustomPicker.UIViewType, context: UIViewRepresentableContext<CustomPicker>) {
        
    }
    
    func makeCoordinator() -> Coordinator {
        return CustomPicker.Coordinator(parent: self)
    }
    
    class Coordinator: NSObject, UIPickerViewDelegate, UIPickerViewDataSource {
        
        private var hour: Int = 0
        private var minute: Int = 0
        var parent: CustomPicker

        init(parent: CustomPicker) {
            self.parent = parent
            let date: Date = self.parent.date
            self.hour = Calendar.current.component(.hour, from: date)
            self.minute = Calendar.current.component(.minute, from: date)
        }
        
        func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
            switch component {
            case 0: return hours.count
            case 1: return 1
            case 2: return minutes.count
            default: return 0
            }
        }
        
        func numberOfComponents(in pickerView: UIPickerView) -> Int {
            return 3
        }
        
        func pickerView(_ pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String? {
            switch component {
            case 0: return hours[row]
            case 1: return ""
            case 2: return minutes[row]
            default: return ""
            }
        }
        
        /// Item Design
        func pickerView(_ pickerView: UIPickerView, viewForRow row: Int, forComponent component: Int, reusing view: UIView?) -> UIView {
            
            switch component {
                
            case 0, 2:
                let view: UIView = UIView(frame: CGRect(x: 0, y: 0, width: 60, height: 60))
                view.backgroundColor = .clear
                
                let label = UILabel(frame: CGRect(x: 0, y: 0, width: view.bounds.width, height: view.bounds.height))
                
                label.textColor = UIColor(red: 119/255, green: 156/255, blue: 255/255, alpha: 1)
                label.textAlignment = .center
                label.font = .systemFont(ofSize: 42, weight: .regular)
                label.text = ":"
                
                view.addSubview(label)
                
                switch component {
                case 0:
                    label.text = hours[row]
                case 2:
                    label.text = minutes[row]
                default: break
                }
                
                return view
            case 1:
                let view: UIView = UIView(frame: CGRect(x: 0, y: 0, width: 20, height: 60))
                view.backgroundColor = .clear
                
                let label = UILabel(frame: CGRect(x: 0, y: 0, width: view.bounds.width, height: view.bounds.height))
                
                label.textColor = UIColor(red: 119/255, green: 156/255, blue: 255/255, alpha: 1)
                label.textAlignment = .center
                label.font = .systemFont(ofSize: 38, weight: .regular)
                label.text = ":"
                
                view.addSubview(label)
                return view
            default:
                return UIView()
            }
        }
        
        /// Container Width
        func pickerView(_ pickerView: UIPickerView, widthForComponent component: Int) -> CGFloat {
            switch component {
            case 0, 2:
                return 60
            case 1:
                return 20
            default:
                return 0
            }
        }
        
        /// Item Height
        func pickerView(_ pickerView: UIPickerView, rowHeightForComponent component: Int) -> CGFloat {
            return 60
        }
        
        func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
            switch component {
            case 0:
                self.hour = Int(hours[row])!
            case 2:
                self.minute = Int(minutes[row])!
            default: break
            }
            self.parent.date = Calendar.current.date(bySettingHour: self.hour, minute: self.minute, second: 0, of: Date())!
        }
        
        
    }
    
}

fileprivate var hours = ["00", "01", "02", "03", "04", "05", "06", "07", "08", "09", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23"]
fileprivate var minutes = ["00", "15", "30", "45"]

```
