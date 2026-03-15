import time

class ChronosWallet:
    def __init__(self, user_id, current_age):
        self.user_id = user_id
        self.max_age = 100
        self.is_alive = True
        
        # คำนวณเวลาชีวิตที่เหลือเป็นวินาที (หน่วยย่อยที่สุด)
        years_left = self.max_age - current_age
        self.balance_seconds = years_left * 365 * 24 * 60 * 60 

    def get_balance_formatted(self):
        """แปลงวินาทีเป็นหน่วย Hr:Min:Sec"""
        hrs = self.balance_seconds // 3600
        mins = (self.balance_seconds % 3600) // 60
        secs = self.balance_seconds % 60
        return f"{int(hrs)} CHRONOS, {int(mins)} Min, {int(secs)} Sec"

    def transfer(self, amount_seconds, recipient_wallet):
        if not self.is_alive:
            print("Error: บัญชีถูกปิดเนื่องจากเจ้าของเสียชีวิต")
            return
        
        if self.balance_seconds >= amount_seconds:
            self.balance_seconds -= amount_seconds
            recipient_wallet.receive(amount_seconds)
            print(f"โอนสำเร็จ: {amount_seconds} วินาที")
        else:
            print("ยอดเงิน (เวลา) ไม่เพียงพอ")

    def receive(self, amount):
        if self.is_alive:
            self.balance_seconds += amount

    def burn_protocol(self):
        """กฎเหล็ก: เมื่อเสียชีวิต เงินทั้งหมดจะถูกทำลาย"""
        self.is_alive = False
        self.balance_seconds = 0
        print(f"SYSTEM: บัญชี {self.user_id} ถูกปิดและเผาเหรียญทิ้งถาวร (The Burn Protocol)")

# --- ตัวอย่างการใช้งาน ---
user1 = ChronosWallet("User_A", 25) # อายุ 25 ปี
print(f"ยอดเริ่มต้น: {user1.get_balance_formatted()}")

# จำลองการโอน 1 ชั่วโมง (3600 วินาที)
user2 = ChronosWallet("User_B", 30)
user1.transfer(3600, user2)

# เมื่อเจ้าของเสียชีวิต
user1.burn_protocol()
print(f"ยอดหลังเสียชีวิต: {user1.get_balance_formatted()}")
