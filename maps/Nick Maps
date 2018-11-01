//
//  main.swift
//  Maps (All)
//
//  Created by Nicholas Hatzis-Schoch on 10/21/18.
//  Copyright © 2018 Slick Games. All rights reserved.
//
import Foundation

struct LinearMap<K: Hashable, V>: CustomStringConvertible{
    var keys = [K]()
    var values = [V]()
    var count: Int = 0
    fileprivate func findKeyIndex(_ k: K)->Int?{
        var index: Int? = nil
        for i in 0..<keys.count{
            if keys[i] == k{
                index = Optional(i)
            }
        }
        return index
    }
    mutating func set(_ k: K, _ v: V){
        if findKeyIndex(k) == nil{
            keys.append(k)
            count += 1
        }
        values.insert(v, at: findKeyIndex(k)!)
    }
    func get(_ k: K)->V?{
        let index = findKeyIndex(k)
        if index != nil{
            return values[index!]
        }
        return nil
    }
    var description: String{
        var desc = ""
        for i in 0..<count{
            desc += "\(keys[i]) : \(values[i])\n"
        }
        return desc
    }
    subscript(_ k: K?)->V?{
        get{
            if k != nil{
                return get(k!)
            }
            return nil
        }
        set(newValue){
            if k != nil{
                set(k!, newValue!)
            }
        }
    }
}

struct BinaryMap<K: Comparable, V>: CustomStringConvertible{
    var keys = [K]()
    var values = [V]()
    var count: Int = 0
    mutating func set(_ k: K, _ v: V){
        let binaryIndex = binarySearch(elements: keys, target: k)
        let linearIndex = linearSearch(elements: keys, target: k)
        if binaryIndex != nil{
            values.insert(v, at: binaryIndex!)
        }
        else{
            if linearIndex != nil{
                values.insert(v, at: linearIndex!)
            }
            else{
                keys.append(k)
                values.append(v)
                count += 1
            }
        }
        
    }
    func get(_ k: K)->V?{
        if binarySearch(elements: keys, target: k) != nil{
            return values[binarySearch(elements: keys, target: k)!]
        }
        if linearSearch(elements: keys, target: k) != nil{
            return values[linearSearch(elements: keys, target: k)!]
        }
        return nil
    }
    func binarySearch<K: Comparable>(elements: [K], target: K)->Int?{
        var lowerIndex = 0;
        var upperIndex = elements.count - 1
        if count == 0 {
            return nil
        }
        while (true) {
            let currentIndex = (lowerIndex + upperIndex)/2
            if(elements[currentIndex] == target) {
                return currentIndex
            } else if (lowerIndex > upperIndex) {
                return nil
            } else {
                if (elements[currentIndex] > target) {
                    upperIndex = currentIndex - 1
                } else {
                    lowerIndex = currentIndex + 1
                }
            }
        }
    }
    func linearSearch<K: Comparable>(elements: [K], target: K)->Int?{
        var index: Int? = nil
        for i in 0..<elements.count{
            if elements[i] == target{
                index = Optional(i)
            }
        }
        return index
    }
    var description: String{
        var desc = ""
        for i in 0..<count{
            desc += "\(keys[i]) : \(values[i])\n"
        }
        return desc
    }
    subscript(_ k: K?)->V?{
        get{
            if k != nil{
                return get(k!)
            }
            return nil
        }
        set(newValue){
            if k != nil{
                set(k!, newValue!)
            }
        }
    }
}

struct HashMap<K: Hashable, V>: CustomStringConvertible{
    var count: Int = 0
    var numberCollisions: Int = 0
    var keys: [K?]
    var values: [V?]
    let initialArraySize: Int
    var lm = LinearMap<K, V>()
    init(initialArraySize: Int = 2000){
        self.keys = [K?](repeating: nil, count: initialArraySize)
        self.values = [V?](repeating: nil, count: initialArraySize)
        self.initialArraySize = initialArraySize
    }
    func getNumberCollisions()->Int{
        return numberCollisions
    }
    func shouldStoreInLinear(_ k: K)->Bool{
        if keys[abs(k.hashValue % initialArraySize)] != nil && keys[abs(k.hashValue % initialArraySize)] != k{
            return true
        }
        return false
    }
    mutating func set(_ k: K, _ v: V){
        if shouldStoreInLinear(k){
            if lm.findKeyIndex(k) == nil{
                count += 1
            }
            lm[k] = v
            numberCollisions += 1
        }
        else{
            if keys[abs(k.hashValue % initialArraySize)] != k{
                count += 1
            }
            keys[abs(k.hashValue % initialArraySize)] = k
            values[abs(k.hashValue % initialArraySize)] = v
        }
    }
    mutating func get(_ k: K)->V?{
        if shouldStoreInLinear(k){
             numberCollisions += 1
            return lm[k]
        }
        return values[abs(k.hashValue % initialArraySize)]
    }
    var description: String{
        var desc = ""
        for i in 0..<keys.count{
            if keys[i] != nil{
                desc += "\(keys[i]!) : \(values[i]!)\n"
            }
        }
        desc += lm.description
        return desc
    }
    subscript(_ k: K?)->V?{
        mutating get{
            if k != nil{
                return get(k!)
            }
            return nil
        }
        set(newValue){
            if k != nil{
                set(k!, newValue!)
            }
        }
    }
}
public class MapTest{ // CHANGE initialArrayValue default in HashMap struct to 2000 before testing.
    var stringList = [String]()
    let chars = Array("abcdefghijklmnopqrrstuvwxyz".characters)
    let MAX_SIZE = 1000
    let NUMBER_PUTS = 1000
    func getRandomInt(range: Int)->Int{
        return Int(arc4random_uniform(UInt32(range)))
    }
    func makeString(length: Int)->String{
        var s = ""
        let numberChars = chars.count
        for _ in 0..<length{
            s.append(chars[getRandomInt(range: numberChars)])
        }
        return s
    }
    func makeStringList(size: Int){
        stringList = [String]()
        for _ in 0..<size{
            stringList.append(makeString(length: 10))
        }
    }
    func testLinearMap()->Bool{
        var value = ""
        var key = ""
        var map = LinearMap<String, String>()
        for i in 0..<NUMBER_PUTS{
            map.set(stringList[i], stringList[i]);
        }
        for index in 0..<NUMBER_PUTS{
            key = stringList[index]
            value = map.get(key)!
            if (!(value == key)){
                return false;
            }
        }
        return true;
    }
    func testBinaryMap()->Bool{
        var value = ""
        var key = ""
        var map = BinaryMap<String, String>()
        for i in 0..<NUMBER_PUTS{
            map.set(stringList[i], stringList[i]);
        }
        for index in 0..<NUMBER_PUTS{
            key = stringList[index]
            value = map.get(key)!
            if (!(value == key)){
                return false;
            }
        }
        return true;
    }
    func testHashMap()->Bool{
        var value = ""
        var key = ""
        var map = HashMap<String, String>()
        for i in 0..<NUMBER_PUTS{
            map.set(stringList[i], stringList[i]);
        }
        for index in 0..<NUMBER_PUTS{
            key = stringList[index]
            value = map.get(key)!
            if (!(value == key)){
                return false;
            }
        }
        print("Collisions: \(map.numberCollisions)")
        return true;
    }
    func doTest(){
        makeStringList(size: MAX_SIZE)
        print("String List Complete")
        print("Linear: \(testLinearMap())")
        print("Binary: \(testBinaryMap())")
        print("Hash: \(testHashMap())")
    }
}

let mt = MapTest()
mt.doTest()

